# Refining Bad Peers on your Initia Validator following these few lines of codes
  
  **Download the Specific AddrBook with "Curl" or "wget":**

      wget <RPC_EndPoint_/AddrBoook>.json


**move the file to your initia directory:**

      mv addrbook.json ~/.initia/config


**Create and Edit the Python Script:**

      nano filter_addrbook.py


**Paste the Following Code into 'filter_addrbook.py':**

    import json

    file_path = 'addrbook.json'  # Adjust the path if the file is in a different location
    with open(file_path, 'r') as file:
          addrbook = json.load(file)
  
      def filter_peers(peers, max_attempts, success_threshold):
          filtered = []
          for peer in peers:
              if peer.get("attempts", 0) <= max_attempts and peer.get("last_success", success_threshold) != success_threshold:
                  filtered.append(peer)
          return filtered

      max_attempts = 50
      success_threshold = "0001-01-01T00:00:00Z"

      filtered_peers = filter_peers(addrbook.get("addrs", []), max_attempts, success_threshold)

      filtered_addrbook = {
          "key": addrbook.get("key"),
          "addrs": filtered_peers
      }

      filtered_file_path = 'filtered_addrbook.json'
      with open(filtered_file_path, 'w') as file:
          json.dump(filtered_addrbook, file, indent=4)

      original_count = len(addrbook.get("addrs", []))
      filtered_count = len(filtered_peers)

      print(f"Original number of peers: {original_count}")
      print(f"Filtered number of peers: {filtered_count}")
      print(f"Filtered addrbook saved to {filtered_file_path}")

Make sure you download the "Addrbook" before running this last line

**Run the Script:**
      
      python3 filter_addrbook.py
      
After running this, Refined Peers are saved in 'filtered_addrbook.json'.

**Open Configuration file with :**

      nano $HOME/.initia/config/config.toml

**Define** 

      AddrBook = " filter_addebook.json"

Run CTRL X,Y and Press **Enter**


Restart your initia validator node:


```sudo systemctl restart initiad.service && sudo journalctl -fu initiad.service -o cat```
            

            
