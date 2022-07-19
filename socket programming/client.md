```python
import socket

HOST = "127.0.0.1"
PORT = 8000

msg = "HI, what's your name?"
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((HOST, PORT))
client.sendall(msg.encode())

serverMsg = client.recv(1024)
print("server return msg : ", serverMsg.decode())
client.close()

```
