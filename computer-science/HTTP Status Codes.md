#course_cs50 

- There are several status codes that servers can send back, indicating the status of the request.
# 1** Informational responses

# 2** Successful responses

| Code | Meaning |
| ---- | ------- |
| 200  | OK      |
## 200 OK

- All is good.
# 3** Redirection messages

| Code | Meaning            |
| ---- | ------------------ |
| 301  | Moved Permanently  |
| 302  | Found              |
| 304  | Not Modified       |
| 307  | Temporary Redirect |

## 301 Moved Permanently

- See what happens when we run the following `curl` command. What we're seeing is that <https://harvard.edu/> is actually an alias for <https://www.harvard.edu/.>

![[Pasted image 20250303175901.png]]


# 4** Client error responses

| Code | Meaning      |
| ---- | ------------ |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not Found    |
| 418  | I'm a Teapot |
## 404 File not found

- Happens when you navigate to a non-existent URL


# 5** Server error responses

| Code | Meaning               |
| ---- | --------------------- |
| 500  | Internal Server Error |
| 503  | Service Unavailable   |