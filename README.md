# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
NAME:AMRITA BS 
REG NO:212225100002

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
<BR>
## Program 
```
import socket
import webbrowser
import os


def send_request(host, port, request):
    with socket.create_connection((host, port), timeout=10) as s:
        s.sendall(request)

        response = b''
        try:
            while True:
                data = s.recv(4096)
                if not data:
                    break
                response += data
        except socket.timeout:
            print("Socket timed out waiting for data")

    return response


def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()

    request = (
        f"POST /upload HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Content-Type: text/html\r\n"
        f"Content-Length: {len(file_data)}\r\n"
        f"Connection: close\r\n"
        f"\r\n"
    ).encode() + file_data

    response = send_request(host, port, request)

    return response


def download_file(host, port, filename):
    request = (
        f"GET /{filename} HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Connection: close\r\n"
        f"\r\n"
    ).encode()

    response = send_request(host, port, request)

    # Separate HTTP headers from webpage content
    if b'\r\n\r\n' not in response:
        print("Invalid HTTP response")
        return

    file_content = response.split(b'\r\n\r\n', 1)[1]

    # Save downloaded webpage
    downloaded_filename = "downloaded_" + filename

    with open(downloaded_filename, 'wb') as file:
        file.write(file_content)

    print("File downloaded successfully.")

    # Open webpage in browser
    file_path = os.path.abspath(downloaded_filename)
    webbrowser.open("file://" + file_path)

    print("Webpage opened in browser.")


if __name__ == "__main__":

    host = 'example.com'
    port = 80

    # Upload HTML webpage
    upload_response = upload_file(host, port, 'example.html')
    print("Upload response:")
    print(upload_response.decode(errors='ignore'))

    # Download HTML webpage
    download_file(host, port, 'index.html')
```
##OUTPUT:
<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/337c0b76-ab00-4f27-83da-20092e6b3bc2" />
<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/f33d4fbd-52e6-43e5-93d5-4eb6c10955e2" />


## Result
Thus the socket for HTTP for web page upload and download created and Executed
