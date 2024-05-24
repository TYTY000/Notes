#getaddrinfo

``` cpp
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>

int getaddrinfo(const char *node,     // e.g. "www.example.com" or IP
                const char *service,  // e.g. "http" or port number
                const struct addrinfo *hints,
                struct addrinfo **res);
```

#socket 
```cpp
#include <sys/types.h>
#include <sys/socket.h>

int socket(int domain, int type, int protocol); 
```
#bind 
一般用于服务端监听前端口的绑定。
```cpp
#include <sys/types.h>
#include <sys/socket.h>

int bind(int sockfd, struct sockaddr *my_addr, int addrlen);
```
