## HTTP response errow:
```python
connection.sendall(b'HTTP/1.0 200 OK\r\nContent-Length: 11\r\nContent-Type: text/html; charset=UTF-8\r\n\r\nHello World\r\n')
```
- code
```python
import socket

HOST = "127.0.0.1"
PORT = 8000
passKey = "你好, my name is Hank !"
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((HOST, PORT))   #using tuple
server.listen(1)
print(f"Server 在{HOST}:{PORT}")
print("Waiting for connection...")
while True:
    conn, addr = server.accept()
    print(f"目前連線的IP :{addr}")
    data = conn.recv(1024)
    #print(type(data))
    print("Client message is : ", data.decode())
    tmp = data.decode() + passKey
    conn.sendall(tmp.encode())
    conn.close()

```
