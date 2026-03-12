# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program

## Program 
client
```
import socket

def download_file(host, port, filename):
    req = f"GET /{filename} HTTP/1.1\r\nHost: {host}\r\nConnection: close\r\n\r\n"
    
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(req.encode())
        response = b""
        while (data := s.recv(4096)):
            response += data

    body = response.split(b"\r\n\r\n", 1)[1]
    out = f"downloaded_{filename}"
    with open(out, "wb") as f:
        f.write(body)

    print(f"Downloaded as {out}")

if __name__ == "__main__":
    download_file("192.168.56.1", 8080, "example.txt")
```
server
```
import socket

HOST = '0.0.0.0'
PORT = 8080

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((HOST, PORT))
server.listen(1)

print("Server listening on port 8080...")

while True:
    conn, addr = server.accept()
    print("Connected by", addr)

    request = conn.recv(1024)

    with open("example.txt", "rb") as f:
        body = f.read()

    response = b"HTTP/1.1 200 OK\r\n\r\n" + body
    conn.sendall(response)

    conn.close()
```
## OUTPUT
<img width="1030" height="194" alt="Screenshot 2026-03-11 180144" src="https://github.com/user-attachments/assets/6cb5ee02-a43e-43b3-95d5-b6e419c72b9b" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
