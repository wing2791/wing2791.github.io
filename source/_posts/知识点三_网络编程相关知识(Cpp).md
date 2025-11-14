---
title: 知识点三_网络编程相关知识(Cpp)
math: true
---
# 网络编程的基础知识，socket的库函数
- 网络通讯是指两台计算机中的程序进行数据传输的过程
- 客户程序（端）：指主动发起通讯的程序
- 服务程序（端/器）：指被动的等待，然后为向它发起通讯的客户端提供服务

- demo1.cpp
- 客户端
```cpp
/*
 * 程序名：demo1.cpp，此程序用于演示socket的客户端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        cout << "Using:./demo1 服务端的IP 服务端的端口\nExample:./demo1 192.168.101.139 5005\n\n";
        return -1;
    }

    // 第1步：创建客户端的socket。
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd == -1)
    {
        perror("socket");
        return -1;
    }

    // 第2步：向服务器发起连接请求。
    struct hostent *h;                     // 用于存放服务端IP的结构体。
    if ((h = gethostbyname(argv[1])) == 0) // 把字符串格式的IP转换成结构体。
    {
        cout << "gethostbyname failed.\n"
             << endl;
        close(sockfd);
        return -1;
    }
    struct sockaddr_in servaddr; // 用于存放服务端IP和端口的结构体。
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    memcpy(&servaddr.sin_addr, h->h_addr, h->h_length); // 指定服务端的IP地址。
    servaddr.sin_port = htons(atoi(argv[2]));           // 指定服务端的通信端口。

    if (connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0) // 向服务端发起连接清求。
    {
        perror("connect");
        close(sockfd);
        return -1;
    }

    // 第3步：与服务端通讯，客户发送一个请求报文后等待服务端的回复，收到回复后，再发下一个请求报文。
    char buffer[1024];
    for (int ii = 0; ii < 3; ii++) // 循环3次，将与服务端进行三次通讯。
    {
        int iret;
        memset(buffer, 0, sizeof(buffer));
        sprintf(buffer, "这是第%d个超级女生，编号%03d。", ii + 1, ii + 1); // 生成请求报文内容。
        // 向服务端发送请求报文。
        if ((iret = send(sockfd, buffer, strlen(buffer), 0)) <= 0)
        {
            perror("send");
            break;
        }
        cout << "发送：" << buffer << endl;

        memset(buffer, 0, sizeof(buffer));
        // 接收服务端的回应报文，如果服务端没有发送回应报文，recv()函数将阻塞等待。
        if ((iret = recv(sockfd, buffer, sizeof(buffer), 0)) <= 0)
        {
            cout << "iret=" << iret << endl;
            break;
        }
        cout << "接收：" << buffer << endl;

        sleep(1);
    }

    // 第4步：关闭socket，释放资源。
    close(sockfd);
}

```

- 服务端
- demo2.cpp
```cpp
/*
 * 程序名：demo2.cpp，此程序用于演示socket通信的服务端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        cout << "Using:./demo2 通讯端口\nExample:./demo2 5005\n\n"; // 端口大于1024，不与其它的重复。
        cout << "注意：运行服务端程序的Linux系统的防火墙必须要开通5005端口。\n";
        cout << "      如果是云服务器，还要开通云平台的访问策略。\n\n";
        return -1;
    }

    // 第1步：创建服务端的socket。
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);
    if (listenfd == -1)
    {
        perror("socket");
        return -1;
    }

    // 第2步：把服务端用于通信的IP和端口绑定到socket上。
    struct sockaddr_in servaddr; // 用于存放服务端IP和端口的数据结构。
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;                // 指定协议。
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY); // 服务端任意网卡的IP都可以用于通讯。
    servaddr.sin_port = htons(atoi(argv[1]));     // 指定通信端口，普通用户只能用1024以上的端口。
    // 绑定服务端的IP和端口。
    if (bind(listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0)
    {
        perror("bind");
        close(listenfd);
        return -1;
    }

    // 第3步：把socket设置为可连接（监听）的状态。
    if (listen(listenfd, 5) != 0)
    {
        perror("listen");
        close(listenfd);
        return -1;
    }

    // 第4步：受理客户端的连接请求，如果没有客户端连上来，accept()函数将阻塞等待。
    int clientfd = accept(listenfd, 0, 0);
    if (clientfd == -1)
    {
        perror("accept");
        close(listenfd);
        return -1;
    }

    cout << "客户端已连接。\n";

    // 第5步：与客户端通信，接收客户端发过来的报文后，回复ok。
    char buffer[1024];
    while (true)
    {
        int iret;
        memset(buffer, 0, sizeof(buffer));
        // 接收客户端的请求报文，如果客户端没有发送请求报文，recv()函数将阻塞等待。
        // 如果客户端已断开连接，recv()函数将返回0,直接退出服务端
        // 如果 iret > 0，表示成功接收到数据，返回的是接收到的字节数。
        // 如果 iret == 0，表示连接已关闭，数据传输结束。
        // 如果 iret < 0，则发生错误。常见错误可能是网络问题或对方异常断开连接。
        if ((iret = recv(clientfd, buffer, sizeof(buffer), 0)) <= 0)
        {
            cout << "iret=" << iret << endl;
            break;
        }
        cout << "接收：" << buffer << endl;

        strcpy(buffer, "ok"); // 生成回应报文内容。
        // 向客户端发送回应报文。
        if ((iret = send(clientfd, buffer, strlen(buffer), 0)) <= 0)
        {
            perror("send");
            break;
        }
        cout << "发送：" << buffer << endl;
    }

    // 第6步：关闭socket，释放资源。
    close(listenfd); // 关闭服务端用于监听的socket。
    close(clientfd); // 关闭客户端连上来的socket。
}

```

- 客户端，封装成类了
demo7.cpp
```cpp
/*
 * 程序名：demo7.cpp，此程序用于演示封装socket通讯的客户端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

class ctcpclient // TCP通讯的客户端类。
{
private:
    int m_clientfd;        // 客户端的socket，-1表示未连接或连接已断开；>=0表示有效的socket。
    string m_ip;           // 服务端的IP/域名。
    unsigned short m_port; // 通讯端口。
public:
    ctcpclient() : m_clientfd(-1) {}

    // 向服务端发起连接请求，成功返回true，失败返回false。
    bool connect(const string &in_ip, const unsigned short in_port)
    {
        if (m_clientfd != -1)
            return false; // 如果socket已连接，直接返回失败。

        m_ip = in_ip;
        m_port = in_port; // 把服务端的IP和端口保存到成员变量中。

        // 第1步：创建客户端的socket。
        if ((m_clientfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
            return false;

        // 第2步：向服务器发起连接请求。
        struct sockaddr_in servaddr; // 用于存放协议、端口和IP地址的结构体。
        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;     // ①协议族，固定填AF_INET。
        servaddr.sin_port = htons(m_port); // ②指定服务端的通信端口。

        struct hostent *h;                                // 用于存放服务端IP地址(大端序)的结构体的指针。
        if ((h = gethostbyname(m_ip.c_str())) == nullptr) // 把域名/主机名/字符串格式的IP转换成结构体。
        {
            // 调用系统库函数加::
            ::close(m_clientfd);
            m_clientfd = -1;
            return false;
        }
        memcpy(&servaddr.sin_addr, h->h_addr, h->h_length); // ③指定服务端的IP(大端序)。

        // 向服务端发起连接清求。
        if (::connect(m_clientfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) == -1)
        {
            ::close(m_clientfd);
            m_clientfd = -1;
            return false;
        }

        return true;
    }

    // 向服务端发送报文，成功返回true，失败返回false。
    bool send(const string &buffer) // buffer不要用const char *，定义为const string&既支持string类型，也支持C风格的字符串，定义成const char*则不支持string
    {
        if (m_clientfd == -1)
            return false; // 如果socket的状态是未连接，直接返回失败。
                          //::send：调用全局的 send 函数，避免在当前作用域或命名空间中出现同名的 send 影响调用。
                          // m_clientfd：是目标客户端的文件描述符。
                          // buffer.data()：获取数据缓冲区的指针，即需要发送的数据的起始地址。
                          // buffer.size()：指定发送的数据长度。
                          // 0：是发送标志位，在这里使用 0 表示默认行为。
        if ((::send(m_clientfd, buffer.data(), buffer.size(), 0)) <= 0)
            return false;

        return true;
    }

    // 接收服务端的报文，成功返回true，失败返回false。
    // buffer-存放接收到的报文的内容，maxlen-本次接收报文的最大长度。
    bool recv(string &buffer, const size_t maxlen)
    {                                                                 // 如果直接操作string对象的内存，必须保证：1)不能越界；2）操作后手动设置数据的大小。
                                                                      // 因为是直接操作string对象的内存，因为传递了首地址，直接操控了string对象的内存，所以需要手动设置数据的大小。如果通过string类的成员函数操控string对象，会自动设置数据的大小。
        buffer.clear();                                               // 清空容器。
        buffer.resize(maxlen);                                        // 设置容器的大小为maxlen。
                                                                      // 这里获取首地址可以用buffer.c_str(),buffer.data()不过这些都是const，&buffer[0]是非const
                                                                      // readn -1失败，0 socker连接已断开，>0成功收到了数据，是数据的大小
        int readn = ::recv(m_clientfd, &buffer[0], buffer.size(), 0); // 直接操作buffer的内存。
        if (readn <= 0)
        {
            buffer.clear();
            return false;
        }                     // 清空string的大小，大小变为0，并释放存储字符内容的空间，否则大小为maxlen
        buffer.resize(readn); // 重置buffer的实际大小。

        return true;
    }

    // 断开与服务端的连接。
    bool close()
    {
        if (m_clientfd == -1)
            return false; // 如果socket的状态是未连接，直接返回失败。

        ::close(m_clientfd);
        m_clientfd = -1;
        return true;
    }

    ~ctcpclient() { close(); }
};

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        cout << "Using:./demo7 服务端的IP 服务端的端口\nExample:./demo7 192.168.101.138 5005\n\n";
        return -1;
    }

    ctcpclient tcpclient;
    if (tcpclient.connect(argv[1], atoi(argv[2])) == false) // 向服务端发起连接请求。
    {
        perror("connect()");
        return -1;
    }

    // 第3步：与服务端通讯，客户发送一个请求报文后等待服务端的回复，收到回复后，再发下一个请求报文。
    string buffer;
    for (int ii = 0; ii < 10; ii++) // 循环3次，将与服务端进行三次通讯。
    {
        buffer = "这是第" + to_string(ii + 1) + "个超级女生，编号" + to_string(ii + 1) + "。";
        // 向服务端发送请求报文。
        if (tcpclient.send(buffer) == false)
        {
            perror("send");
            break;
        }
        cout << "发送：" << buffer << endl;

        // 接收服务端的回应报文，如果服务端没有发送回应报文，recv()函数将阻塞等待。
        if (tcpclient.recv(buffer, 1024) == false)
        {
            perror("recv()");
            break;
        }
        cout << "接收：" << buffer << endl;

        sleep(1);
    }
}

```

- 服务端，封装成类了
demo8.cpp
```cpp
/*
 * 程序名：demo8.cpp，此程序用于演示封装socket通讯的服务端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

class ctcpserver // TCP通讯的服务端类。
{
private:
    int m_listenfd;        // 监听的socket，-1表示未初始化。
    int m_clientfd;        // 客户端连上来的socket，-1表示客户端未连接。
    string m_clientip;     // 客户端字符串格式的IP。
    unsigned short m_port; // 服务端用于通讯的端口。
public:
    ctcpserver() : m_listenfd(-1), m_clientfd(-1) {}

    // 初始化服务端用于监听的socket。
    bool initserver(const unsigned short in_port)
    {
        // 第1步：创建服务端的socket。
        if ((m_listenfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
            return false;

        m_port = in_port;

        // 第2步：把服务端用于通信的IP和端口绑定到socket上。
        struct sockaddr_in servaddr; // 用于存放协议、端口和IP地址的结构体。
        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;                // ①协议族，固定填AF_INET。
        servaddr.sin_port = htons(m_port);            // ②指定服务端的通信端口。
        servaddr.sin_addr.s_addr = htonl(INADDR_ANY); // ③如果操作系统有多个IP，全部的IP都可以用于通讯。

        // 绑定服务端的IP和端口（为socket分配IP和端口）。
        if (bind(m_listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) == -1)
        {
            close(m_listenfd);
            m_listenfd = -1;
            return false;
        }

        // 第3步：把socket设置为可连接（监听）的状态。listen(m_listenfd, 5) 将套接字 m_listenfd 设置为被动监听状态，以便接受客户端连接。5 表示等待连接的队列大小，即最多允许 5 个连接排队
        if (listen(m_listenfd, 5) == -1)
        {
            close(m_listenfd);
            m_listenfd = -1;
            return false;
        }

        return true;
    }

    // 受理客户端的连接（从已连接的客户端中取出一个客户端），
    // 如果没有已连接的客户端，accept()函数将阻塞等待。
    bool accept()
    {
        struct sockaddr_in caddr;          // 客户端的地址信息。
        socklen_t addrlen = sizeof(caddr); // struct sockaddr_in的大小。
                                           // ::accept()的第二第三个参数为空，表示不关心客户端的地址信息，如果想要知道客户端的地址信息，第二个参数传入客户端地址信息的地址，第三个参数传入客户端地址的大小的地址
        if ((m_clientfd = ::accept(m_listenfd, (struct sockaddr *)&caddr, &addrlen)) == -1)
            return false;

        m_clientip = inet_ntoa(caddr.sin_addr); // 把客户端的地址从大端序转换成字符串。无论在大端序还是小端序系统上，inet_ntoa 都可以正确将 caddr.sin_addr 转换成字符串格式的 IP 地址

        return true;
    }

    // 获取客户端的IP(字符串格式)。
    const string &clientip() const
    {
        return m_clientip;
    }

    // 向对端发送报文，成功返回true，失败返回false。
    bool send(const string &buffer)
    {
        if (m_clientfd == -1)
            return false;

        if ((::send(m_clientfd, buffer.data(), buffer.size(), 0)) <= 0)
            return false;

        return true;
    }

    // 接收对端的报文，成功返回true，失败返回false。
    // buffer-存放接收到的报文的内容，maxlen-本次接收报文的最大长度。
    bool recv(string &buffer, const size_t maxlen)
    {
        buffer.clear();                                               // 清空容器。
        buffer.resize(maxlen);                                        // 设置容器的大小为maxlen。
        int readn = ::recv(m_clientfd, &buffer[0], buffer.size(), 0); // 直接操作buffer的内存。
        if (readn <= 0)
        {
            buffer.clear();
            return false;
        }
        buffer.resize(readn); // 重置buffer的实际大小。

        return true;
    }

    // 关闭监听的socket。
    bool closelisten()
    {
        if (m_listenfd == -1)
            return false;

        ::close(m_listenfd);
        m_listenfd = -1;
        return true;
    }

    // 关闭客户端连上来的socket。
    bool closeclient()
    {
        if (m_clientfd == -1)
            return false;

        ::close(m_clientfd);
        m_clientfd = -1;
        return true;
    }

    ~ctcpserver()
    {
        closelisten();
        closeclient();
    }
};

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        cout << "Using:./demo8 通讯端口\nExample:./demo8 5005\n\n"; // 端口大于1024，不与其它的重复。
        cout << "注意：运行服务端程序的Linux系统的防火墙必须要开通5005端口。\n";
        cout << "      如果是云服务器，还要开通云平台的访问策略。\n\n";
        return -1;
    }

    ctcpserver tcpserver;
    if (tcpserver.initserver(atoi(argv[1])) == false) // 初始化服务端用于监听的socket。
    {
        perror("initserver()");
        return -1;
    }

    // 受理客户端的连接（从已连接的客户端中取出一个客户端），
    // 如果没有已连接的客户端，accept()函数将阻塞等待。
    if (tcpserver.accept() == false)
    {
        perror("accept()");
        return -1;
    }
    cout << "客户端已连接(" << tcpserver.clientip() << ")。\n";

    string buffer;
    while (true)
    {
        // 接收对端的报文，如果对端没有发送报文，recv()函数将阻塞等待。
        if (tcpserver.recv(buffer, 1024) == false)
        {
            perror("recv()");
            break;
        }
        cout << "接收：" << buffer << endl;

        buffer = "ok";
        if (tcpserver.send(buffer) == false) // 向对端发送报文。
        {
            perror("send");
            break;
        }
        cout << "发送：" << buffer << endl;
    }
}

```

- 多线程的服务端
demo10.cpp
```cpp
/*
 * 程序名：demo10.cpp，此程序用于演示多进程的socket服务端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

class ctcpserver // TCP通讯的服务端类。
{
private:
    int m_listenfd;        // 监听的socket，-1表示未初始化。
    int m_clientfd;        // 客户端连上来的socket，-1表示客户端未连接。
    string m_clientip;     // 客户端字符串格式的IP。
    unsigned short m_port; // 服务端用于通讯的端口。
public:
    ctcpserver() : m_listenfd(-1), m_clientfd(-1) {}

    // 初始化服务端用于监听的socket。
    bool initserver(const unsigned short in_port)
    {
        // 第1步：创建服务端的socket。
        if ((m_listenfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
            return false;

        m_port = in_port;

        // 第2步：把服务端用于通信的IP和端口绑定到socket上。
        struct sockaddr_in servaddr; // 用于存放协议、端口和IP地址的结构体。
        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;                // ①协议族，固定填AF_INET。
        servaddr.sin_port = htons(m_port);            // ②指定服务端的通信端口。
        servaddr.sin_addr.s_addr = htonl(INADDR_ANY); // ③如果操作系统有多个IP，全部的IP都可以用于通讯。

        // 绑定服务端的IP和端口（为socket分配IP和端口）。
        if (bind(m_listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) == -1)
        {
            close(m_listenfd);
            m_listenfd = -1;
            return false;
        }

        // 第3步：把socket设置为可连接（监听）的状态。
        if (listen(m_listenfd, 5) == -1)
        {
            close(m_listenfd);
            m_listenfd = -1;
            return false;
        }

        return true;
    }

    // 受理客户端的连接（从已连接的客户端中取出一个客户端），
    // 如果没有已连接的客户端，accept()函数将阻塞等待。
    bool accept()
    {
        struct sockaddr_in caddr;          // 客户端的地址信息。
        socklen_t addrlen = sizeof(caddr); // struct sockaddr_in的大小。
        if ((m_clientfd = ::accept(m_listenfd, (struct sockaddr *)&caddr, &addrlen)) == -1)
            return false;

        m_clientip = inet_ntoa(caddr.sin_addr); // 把客户端的地址从大端序转换成字符串。

        return true;
    }

    // 获取客户端的IP(字符串格式)。
    const string &clientip() const
    {
        return m_clientip;
    }

    // 向对端发送报文，成功返回true，失败返回false。
    bool send(const string &buffer)
    {
        if (m_clientfd == -1)
            return false;

        if ((::send(m_clientfd, buffer.data(), buffer.size(), 0)) <= 0)
            return false;

        return true;
    }

    // 接收对端的报文，成功返回true，失败返回false。
    // buffer-存放接收到的报文的内容，maxlen-本次接收报文的最大长度。
    bool recv(string &buffer, const size_t maxlen)
    {
        buffer.clear();                                               // 清空容器。
        buffer.resize(maxlen);                                        // 设置容器的大小为maxlen。
        int readn = ::recv(m_clientfd, &buffer[0], buffer.size(), 0); // 直接操作buffer的内存。
        if (readn <= 0)
        {
            buffer.clear();
            return false;
        }
        buffer.resize(readn); // 重置buffer的实际大小。

        return true;
    }

    // 关闭监听的socket。
    bool closelisten()
    {
        if (m_listenfd == -1)
            return false;

        ::close(m_listenfd);
        m_listenfd = -1;
        return true;
    }

    // 关闭客户端连上来的socket。
    bool closeclient()
    {
        if (m_clientfd == -1)
            return false;

        ::close(m_clientfd);
        m_clientfd = -1;
        return true;
    }

    ~ctcpserver()
    {
        closelisten();
        closeclient();
    }
};

ctcpserver tcpserver;

void FathEXIT(int sig); // 父进程的信号处理函数。
void ChldEXIT(int sig); // 子进程的信号处理函数。

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        cout << "Using:./demo10 通讯端口\nExample:./demo10 5005\n\n";
        cout << "注意：运行服务端程序的Linux系统的防火墙必须要开通5005端口。\n";
        cout << "      如果是云服务器，还要开通云平台的访问策略。\n\n";
        return -1;
    }

    // 忽略全部的信号，不希望被打扰。顺便解决了僵尸进程的问题。
    for (int ii = 1; ii <= 64; ii++)
        signal(ii, SIG_IGN);

    // 设置信号,在shell状态下可用 "kill 进程号" 或 "Ctrl+c" 正常终止些进程
    // 但请不要用 "kill -9 +进程号" 强行终止
    signal(SIGTERM, FathEXIT);
    signal(SIGINT, FathEXIT); // SIGTERM 15 SIGINT 2

    if (tcpserver.initserver(atoi(argv[1])) == false) // 初始化服务端用于监听的socket。
    {
        perror("initserver()");
        return -1;
    }

    while (true)
    {
        // 受理客户端的连接（从已连接的客户端中取出一个客户端），
        // 如果没有已连接的客户端，accept()函数将阻塞等待。
        if (tcpserver.accept() == false)
        {
            perror("accept()");
            return -1;
        }

        int pid = fork();
        if (pid == -1)
        {
            perror("fork()");
            return -1;
        } // 系统资源不足。
        if (pid > 0)
        {                            // 父进程。
            tcpserver.closeclient(); // 父进程关闭客户端连接的socket。
            continue;                // 父进程返回到循环开始的位置，继续受理客户端的连接。
        }

        tcpserver.closelisten(); // 子进程关闭监听的socket。

        // 子进程需要重新设置信号。
        signal(SIGTERM, ChldEXIT); // 子进程的退出函数与父进程不一样。
        signal(SIGINT, SIG_IGN);   // 子进程不需要捕获SIGINT信号。

        // 子进程负责与客户端进行通讯。
        cout << "客户端已连接(" << tcpserver.clientip() << ")。\n";

        string buffer;
        while (true)
        {
            // 接收对端的报文，如果对端没有发送报文，recv()函数将阻塞等待。
            if (tcpserver.recv(buffer, 1024) == false)
            {
                // 客户端停止发送消息时，会输出recv(): Invalid argument
                perror("recv()");
                break;
            }
            cout << "接收：" << buffer << endl;

            buffer = "ok";
            if (tcpserver.send(buffer) == false) // 向对端发送报文。
            {
                perror("send");
                break;
            }
            cout << "发送：" << buffer << endl;
        }

        return 0; // 子进程一定要退出，否则又会回到accept()函数的位置。
    }
}

// 父进程的信号处理函数。
void FathEXIT(int sig)
{
    // 以下代码是为了防止信号处理函数在执行的过程中再次被信号中断。
    signal(SIGINT, SIG_IGN);
    signal(SIGTERM, SIG_IGN);

    cout << "父进程退出，sig=" << sig << endl;

    kill(0, SIGTERM); // 向全部的子进程发送15的信号，通知它们退出。

    // 在这里增加释放资源的代码（全局的资源）。
    tcpserver.closelisten(); // 父进程关闭监听的socket。

    exit(0);
}

// 子进程的信号处理函数。
void ChldEXIT(int sig)
{
    // 以下代码是为了防止信号处理函数在执行的过程中再次被信号中断。
    signal(SIGINT, SIG_IGN);
    signal(SIGTERM, SIG_IGN);

    cout << "子进程" << getpid() << "退出，sig=" << sig << endl;

    // 在这里增加释放资源的代码（只释放子进程的资源）。
    tcpserver.closeclient(); // 子进程关闭客户端连上来的socket。

    exit(0);
}

```

- demo11.cpp
- 实现文件传输功能-客户端
```cpp
/*
 * 程序名：demo11.cpp，此程序用于演示文件传输的客户端。
 */
#include <iostream>
#include <fstream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

class ctcpclient // TCP通讯的客户端类。
{
private:
    int m_clientfd;        // 客户端的socket，-1表示未连接或连接已断开；>=0表示有效的socket。
    string m_ip;           // 服务端的IP/域名。
    unsigned short m_port; // 通讯端口。
public:
    ctcpclient() : m_clientfd(-1) {}

    // 向服务端发起连接请求，成功返回true，失败返回false。
    bool connect(const string &in_ip, const unsigned short in_port)
    {
        if (m_clientfd != -1)
            return false; // 如果socket已连接，直接返回失败。

        m_ip = in_ip;
        m_port = in_port; // 把服务端的IP和端口保存到成员变量中。

        // 第1步：创建客户端的socket。
        if ((m_clientfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
            return false;

        // 第2步：向服务器发起连接请求。
        struct sockaddr_in servaddr; // 用于存放协议、端口和IP地址的结构体。
        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;     // ①协议族，固定填AF_INET。
        servaddr.sin_port = htons(m_port); // ②指定服务端的通信端口。

        struct hostent *h;                                // 用于存放服务端IP地址(大端序)的结构体的指针。
        if ((h = gethostbyname(m_ip.c_str())) == nullptr) // 把域名/主机名/字符串格式的IP转换成结构体。
        {
            ::close(m_clientfd);
            m_clientfd = -1;
            return false;
        }
        memcpy(&servaddr.sin_addr, h->h_addr, h->h_length); // ③指定服务端的IP(大端序)。

        // 向服务端发起连接清求。
        if (::connect(m_clientfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) == -1)
        {
            ::close(m_clientfd);
            m_clientfd = -1;
            return false;
        }

        return true;
    }

    // 向服务端发送报文（字符串），成功返回true，失败返回false。
    bool send(const string &buffer) // buffer不要用const char *
    {
        if (m_clientfd == -1)
            return false; // 如果socket的状态是未连接，直接返回失败。

        if ((::send(m_clientfd, buffer.data(), buffer.size(), 0)) <= 0)
            return false;

        return true;
    }

    // 向服务端发送报文（二进制数据），成功返回true，失败返回false。
    bool send(void *buffer, const size_t size)
    {
        if (m_clientfd == -1)
            return false; // 如果socket的状态是未连接，直接返回失败。

        if ((::send(m_clientfd, buffer, size, 0)) <= 0)
            return false;

        return true;
    }

    // 接收服务端的报文，成功返回true，失败返回false。
    // buffer-存放接收到的报文的内容，maxlen-本次接收报文的最大长度。
    bool recv(string &buffer, const size_t maxlen)
    {                                                                 // 如果直接操作string对象的内存，必须保证：1)不能越界；2）操作后手动设置数据的大小。
        buffer.clear();                                               // 清空容器。
        buffer.resize(maxlen);                                        // 设置容器的大小为maxlen。
        int readn = ::recv(m_clientfd, &buffer[0], buffer.size(), 0); // 直接操作buffer的内存。
        if (readn <= 0)
        {
            buffer.clear();
            return false;
        }
        buffer.resize(readn); // 重置buffer的实际大小。

        return true;
    }

    // 断开与服务端的连接。
    bool close()
    {
        if (m_clientfd == -1)
            return false; // 如果socket的状态是未连接，直接返回失败。

        ::close(m_clientfd);
        m_clientfd = -1;
        return true;
    }

    // 向服务端发送文件内容。
    bool sendfile(const string &filename, const size_t filesize)
    {
        // 以二进制的方式打开文件。
        ifstream fin(filename, ios::binary);
        if (fin.is_open() == false)
        {
            cout << "打开文件" << filename << "失败。\n";
            return false;
        }

        int onread = 0;     // 每次调用fin.read()时打算读取的字节数。  每次应搬砖头数。
        int totalbytes = 0; // 从文件中已读取的字节总数。 已搬砖头数。
        char buffer[4096];  // 存放读取数据的buffer。     每次搬七块砖头。

        while (true)
        {
            memset(buffer, 0, sizeof(buffer));

            // 计算本次应该读取的字节数，如果剩余的数据超过4096字节，就读4096字节。
            if (filesize - totalbytes > 4096)
                onread = 4096;
            else
                onread = filesize - totalbytes;

            // 从文件中读取数据。
            fin.read(buffer, onread);

            // 把读取到的数据发送给对端。
            if (send(buffer, onread) == false)
                return false;

            // 计算文件已读取的字节总数，如果文件已读完，跳出循环。
            totalbytes = totalbytes + onread;

            if (totalbytes == filesize)
                break;
        }

        return true;
    }

    ~ctcpclient() { close(); }
};

int main(int argc, char *argv[])
{
    if (argc != 5)
    {
        cout << "Using:./demo11 服务端的IP 服务端的端口 文件名 文件大小\n";
        cout << "Example:./demo11 192.168.101.138 5005 aaa.txt 2424\n\n";
        return -1;
    }

    ctcpclient tcpclient;
    if (tcpclient.connect(argv[1], atoi(argv[2])) == false) // 向服务端发起连接请求。
    {
        perror("connect()");
        return -1;
    }

    // 以下是发送文件的流程。
    // 1）把待传输文件名和文件的大小告诉服务端。
    // 定义文件信息的结构体。
    struct st_fileinfo
    {
        char filename[256]; // 文件名。
        int filesize;       // 文件大小。
    } fileinfo;
    memset(&fileinfo, 0, sizeof(fileinfo));
    strcpy(fileinfo.filename, argv[3]); // 文件名。
    fileinfo.filesize = atoi(argv[4]);  // 文件大小。
    // 把文件信息的结构体发送给服务端。
    if (tcpclient.send(&fileinfo, sizeof(fileinfo)) == false)
    {
        perror("send");
        return -1;
    }
    cout << "发送文件信息的结构体" << fileinfo.filename << "(" << fileinfo.filesize << ")。" << endl;

    // 2）等待服务端的确认报文（文件名和文件的大小的确认）。
    string buffer;
    if (tcpclient.recv(buffer, 2) == false)
    {
        perror("recv()");
        return -1;
    }
    if (buffer != "ok")
    {
        cout << "服务端没有回复ok。\n";
        return -1;
    }

    // 3）发送文件内容。
    if (tcpclient.sendfile(fileinfo.filename, fileinfo.filesize) == false)
    {
        perror("sendfile()");
        return -1;
    }

    // 4）等待服务端的确认报文（服务端已接收完文件）。
    if (tcpclient.recv(buffer, 2) == false)
    {
        perror("recv()");
        return -1;
    }
    if (buffer != "ok")
    {
        cout << "发送文件内容失败。\n";
        return -1;
    }

    cout << "发送文件内容成功。\n";
}
```

- demo12.cpp
- 实现文件传输功能-服务端
```cpp
/*
 * 程序名：demo12.cpp，此程序用于演示文件传输的服务端。
 */
#include <iostream>
#include <fstream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

class ctcpserver // TCP通讯的服务端类。
{
private:
    int m_listenfd;        // 监听的socket，-1表示未初始化。
    int m_clientfd;        // 客户端连上来的socket，-1表示客户端未连接。
    string m_clientip;     // 客户端字符串格式的IP。
    unsigned short m_port; // 服务端用于通讯的端口。
public:
    ctcpserver() : m_listenfd(-1), m_clientfd(-1) {}

    // 初始化服务端用于监听的socket。
    bool initserver(const unsigned short in_port)
    {
        // 第1步：创建服务端的socket。
        if ((m_listenfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
            return false;

        m_port = in_port;

        // 第2步：把服务端用于通信的IP和端口绑定到socket上。
        struct sockaddr_in servaddr; // 用于存放协议、端口和IP地址的结构体。
        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;                // ①协议族，固定填AF_INET。
        servaddr.sin_port = htons(m_port);            // ②指定服务端的通信端口。
        servaddr.sin_addr.s_addr = htonl(INADDR_ANY); // ③如果操作系统有多个IP，全部的IP都可以用于通讯。

        // 绑定服务端的IP和端口（为socket分配IP和端口）。
        if (bind(m_listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) == -1)
        {
            close(m_listenfd);
            m_listenfd = -1;
            return false;
        }

        // 第3步：把socket设置为可连接（监听）的状态。
        if (listen(m_listenfd, 5) == -1)
        {
            close(m_listenfd);
            m_listenfd = -1;
            return false;
        }

        return true;
    }

    // 受理客户端的连接（从已连接的客户端中取出一个客户端），
    // 如果没有已连接的客户端，accept()函数将阻塞等待。
    bool accept()
    {
        struct sockaddr_in caddr;          // 客户端的地址信息。
        socklen_t addrlen = sizeof(caddr); // struct sockaddr_in的大小。
        if ((m_clientfd = ::accept(m_listenfd, (struct sockaddr *)&caddr, &addrlen)) == -1)
            return false;

        m_clientip = inet_ntoa(caddr.sin_addr); // 把客户端的地址从大端序转换成字符串。

        return true;
    }

    // 获取客户端的IP(字符串格式)。
    const string &clientip() const
    {
        return m_clientip;
    }

    // 向对端发送报文，成功返回true，失败返回false。
    bool send(const string &buffer)
    {
        if (m_clientfd == -1)
            return false;

        if ((::send(m_clientfd, buffer.data(), buffer.size(), 0)) <= 0)
            return false;

        return true;
    }

    // 接收对端的报文（字符串），成功返回true，失败返回false。
    // buffer-存放接收到的报文的内容，maxlen-本次接收报文的最大长度。
    bool recv(string &buffer, const size_t maxlen)
    {
        buffer.clear();                                               // 清空容器。
        buffer.resize(maxlen);                                        // 设置容器的大小为maxlen。
        int readn = ::recv(m_clientfd, &buffer[0], buffer.size(), 0); // 直接操作buffer的内存。
        if (readn <= 0)
        {
            buffer.clear();
            return false;
        }
        buffer.resize(readn); // 重置buffer的实际大小。

        return true;
    }

    // 接收客户端的报文（二进制数据），成功返回true，失败返回false。
    // buffer-存放接收到的报文的内容，size-本次接收报文的最大长度。
    bool recv(void *buffer, const size_t size)
    {
        if (::recv(m_clientfd, buffer, size, 0) <= 0)
            return false;

        return true;
    }

    // 关闭监听的socket。
    bool closelisten()
    {
        if (m_listenfd == -1)
            return false;

        ::close(m_listenfd);
        m_listenfd = -1;
        return true;
    }

    // 关闭客户端连上来的socket。
    bool closeclient()
    {
        if (m_clientfd == -1)
            return false;

        ::close(m_clientfd);
        m_clientfd = -1;
        return true;
    }

    // 接收文件内容。
    bool recvfile(const string &filename, const size_t filesize)
    {
        ofstream fout;
        fout.open(filename, ios::binary);
        if (fout.is_open() == false)
        {
            cout << "打开文件" << filename << "失败。\n";
            return false;
        }

        int totalbytes = 0; // 已接收文件的总字节数。
        int onread = 0;     // 本次打算接收的字节数。
        char buffer[4096];  // 接收文件内容的缓冲区。4096可以更改，主要看磁盘情况，现在一般是1000-5000，这里是读取1页，大概4K

        while (true)
        {
            // 计算本次应该接收的字节数。
            if (filesize - totalbytes > 4096)
                onread = 4096;
            else
                onread = filesize - totalbytes;

            // 接收文件内容。
            if (recv(buffer, onread) == false)
                return false;

            // 把接收到的内容写入文件。
            fout.write(buffer, onread);

            // 计算已接收文件的总字节数，如果文件接收完，跳出循环。
            totalbytes = totalbytes + onread;

            if (totalbytes == filesize)
                break;
        }

        return true;
    }

    ~ctcpserver()
    {
        closelisten();
        closeclient();
    }
};

ctcpserver tcpserver;

void FathEXIT(int sig); // 父进程的信号处理函数。
void ChldEXIT(int sig); // 子进程的信号处理函数。

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        cout << "Using:./demo12 通讯端口 文件存放的目录\n";
        cout << "Example:./demo12 5005 /tmp\n\n";
        cout << "注意：运行服务端程序的Linux系统的防火墙必须要开通5005端口。\n";
        cout << "      如果是云服务器，还要开通云平台的访问策略。\n\n";
        return -1;
    }

    // 忽略全部的信号，不希望被打扰。顺便解决了僵尸进程的问题。
    for (int ii = 1; ii <= 64; ii++)
        signal(ii, SIG_IGN);

    // 设置信号,在shell状态下可用 "kill 进程号" 或 "Ctrl+c" 正常终止些进程
    // 但请不要用 "kill -9 +进程号" 强行终止
    signal(SIGTERM, FathEXIT);
    signal(SIGINT, FathEXIT); // SIGTERM 15 SIGINT 2

    if (tcpserver.initserver(atoi(argv[1])) == false) // 初始化服务端用于监听的socket。
    {
        perror("initserver()");
        return -1;
    }

    while (true)
    {
        // 受理客户端的连接（从已连接的客户端中取出一个客户端），
        // 如果没有已连接的客户端，accept()函数将阻塞等待。
        if (tcpserver.accept() == false)
        {
            perror("accept()");
            return -1;
        }

        int pid = fork();
        if (pid == -1)
        {
            perror("fork()");
            return -1;
        } // 系统资源不足。
        if (pid > 0)
        {                            // 父进程。
            tcpserver.closeclient(); // 父进程关闭客户端连接的socket。
            continue;                // 父进程返回到循环开始的位置，继续受理客户端的连接。
        }

        tcpserver.closelisten(); // 子进程关闭监听的socket。

        // 子进程需要重新设置信号。
        signal(SIGTERM, ChldEXIT); // 子进程的退出函数与父进程不一样。
        signal(SIGINT, SIG_IGN);   // 子进程不需要捕获SIGINT信号。

        // 子进程负责与客户端进行通讯。
        cout << "客户端已连接(" << tcpserver.clientip() << ")。\n";

        // 以下是接收文件的流程。
        // 1）接收文件名和文件大小信息。
        // 定义文件信息的结构体。
        struct st_fileinfo
        {
            char filename[256]; // 文件名。
            int filesize;       // 文件大小。
        } fileinfo;
        memset(&fileinfo, 0, sizeof(fileinfo));
        // 用结构体存放接收报文的内容。
        if (tcpserver.recv(&fileinfo, sizeof(fileinfo)) == false)
        {
            perror("recv()");
            return -1;
        }
        cout << "文件信息结构体" << fileinfo.filename << "(" << fileinfo.filesize << ")。" << endl;

        // 2）给客户端回复确认报文，表示客户端可以发送文件了。
        if (tcpserver.send("ok") == false)
        {
            perror("send");
            break;
        }

        // 3）接收文件内容。  string   char * + const char * + char *,这里char*不能拼接，其中一个转换为string就可以拼接了
        if (tcpserver.recvfile(string(argv[2]) + "/" + fileinfo.filename, fileinfo.filesize) == false)
        {
            cout << "接收文件内容失败。\n";
            return -1;
        }

        cout << "接收文件内容成功。\n";

        // 4）给客户端回复确认报文，表示文件已接收成功。
        tcpserver.send("ok");

        return 0; // 子进程一定要退出，否则又会回到accept()函数的位置。
    }
}

// 父进程的信号处理函数。
void FathEXIT(int sig)
{
    // 以下代码是为了防止信号处理函数在执行的过程中再次被信号中断。
    signal(SIGINT, SIG_IGN);
    signal(SIGTERM, SIG_IGN);

    cout << "父进程退出，sig=" << sig << endl;

    kill(0, SIGTERM); // 向全部的子进程发送15的信号，通知它们退出。

    // 在这里增加释放资源的代码（全局的资源）。
    tcpserver.closelisten(); // 父进程关闭监听的socket。

    exit(0);
}

// 子进程的信号处理函数。
void ChldEXIT(int sig)
{
    // 以下代码是为了防止信号处理函数在执行的过程中再次被信号中断。
    signal(SIGINT, SIG_IGN);
    signal(SIGTERM, SIG_IGN);

    cout << "子进程" << getpid() << "退出，sig=" << sig << endl;

    // 在这里增加释放资源的代码（只释放子进程的资源）。
    tcpserver.closeclient(); // 子进程关闭客户端连上来的socket。

    exit(0);
}
```

# 网络通讯的原理


- demo1.cpp
- 和下面的demo2.cpp结合，能用来测试四次挥手的情况
- 如果客户端和服务端完成三次握手后，如果客户端断开连接后，会有个2MSL的时间的等待，客户端影响不大，只是那个端口不能用了，但如果是服务端主动断开连接后，服务端因为需要服务多个客户端，无法及时重启使用指定端口，会导致有问题，如果想要服务端主动断开并且也能立刻启用端口需要加代码,放在bind函数前
	```cpp
	    int opt = 1;
	    setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));
	```

```cpp
/*
 * 程序名：demo1.cpp，此程序用于演示socket的客户端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        cout << "Using:./demo1 服务端的IP 服务端的端口\nExample:./demo1 192.168.101.139 5005\n\n";
        return -1;
    }

    // 第1步：创建客户端的socket。
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd == -1)
    {
        perror("socket");
        return -1;
    }

    // 第2步：向服务器发起连接请求。
    struct hostent *h;                     // 用于存放服务端IP的结构体。
    if ((h = gethostbyname(argv[1])) == 0) // 把字符串格式的IP转换成结构体。
    {
        cout << "gethostbyname failed.\n"
             << endl;
        close(sockfd);
        return -1;
    }
    struct sockaddr_in servaddr; // 用于存放服务端IP和端口的结构体。
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    memcpy(&servaddr.sin_addr, h->h_addr, h->h_length); // 指定服务端的IP地址。
    servaddr.sin_port = htons(atoi(argv[2]));           // 指定服务端的通信端口。

    if (connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0) // 向服务端发起连接清求。
    {
        perror("connect");
        close(sockfd);
        return -1;
    }

    // sleep(100);

    // 第3步：与服务端通讯，客户发送一个请求报文后等待服务端的回复，收到回复后，再发下一个请求报文。
    char buffer[1024];
    for (int ii = 0; ii < 3; ii++) // 循环3次，将与服务端进行三次通讯。
    {
        int iret;
        memset(buffer, 0, sizeof(buffer));
        sprintf(buffer, "这是第%d个超级女生，编号%03d。", ii + 1, ii + 1); // 生成请求报文内容。
        // 向服务端发送请求报文。
        if ((iret = send(sockfd, buffer, strlen(buffer), 0)) <= 0)
        {
            perror("send");
            break;
        }
        cout << "发送：" << buffer << endl;

        memset(buffer, 0, sizeof(buffer));
        // 接收服务端的回应报文，如果服务端没有发送回应报文，recv()函数将阻塞等待。
        if ((iret = recv(sockfd, buffer, sizeof(buffer), 0)) <= 0)
        {
            cout << "iret=" << iret << endl;
            break;
        }
        cout << "接收：" << buffer << endl;

        sleep(1);
    }

    // 第4步：关闭socket，释放资源。
    close(sockfd);
}
```

- demo2.cpp
- 和上面的demo1.cpp结合，能用来测试四次挥手的情况

```cpp
/*
 * 程序名：demo2.cpp，此程序用于演示socket通信的服务端
 */
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netdb.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
using namespace std;

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        cout << "Using:./demo2 通讯端口\nExample:./demo2 5005\n\n"; // 端口大于1024，不与其它的重复。
        cout << "注意：运行服务端程序的Linux系统的防火墙必须要开通5005端口。\n";
        cout << "      如果是云服务器，还要开通云平台的访问策略。\n\n";
        return -1;
    }

    // 第1步：创建服务端的socket。
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);
    if (listenfd == -1)
    {
        perror("socket");
        return -1;
    }

    // 第2步：把服务端用于通信的IP和端口绑定到socket上。
    struct sockaddr_in servaddr; // 用于存放服务端IP和端口的数据结构。
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;                // 指定协议。
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY); // 服务端任意网卡的IP都可以用于通讯。
    servaddr.sin_port = htons(atoi(argv[1]));     // 指定通信端口，普通用户只能用1024以上的端口。

    // int opt = 1;
    // setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));

    // 绑定服务端的IP和端口。
    if (bind(listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0)
    {
        perror("bind");
        close(listenfd);
        return -1;
    }

    // 第3步：把socket设置为可连接（监听）的状态。
    if (listen(listenfd, 5) != 0)
    {
        perror("listen");
        close(listenfd);
        return -1;
    }

    // 第4步：受理客户端的连接请求，如果没有客户端连上来，accept()函数将阻塞等待。
    int clientfd = accept(listenfd, 0, 0);
    if (clientfd == -1)
    {
        perror("accept");
        close(listenfd);
        return -1;
    }

    cout << "客户端已连接。\n";

    // close(listenfd);
    // close(clientfd);
    // return 0;

    // 第5步：与客户端通信，接收客户端发过来的报文后，回复ok。
    char buffer[1024];
    while (true)
    {
        int iret;
        memset(buffer, 0, sizeof(buffer));
        // 接收客户端的请求报文，如果客户端没有发送请求报文，recv()函数将阻塞等待。
        // 如果客户端已断开连接，recv()函数将返回0,直接退出服务端
        // 如果 iret > 0，表示成功接收到数据，返回的是接收到的字节数。
        // 如果 iret == 0，表示连接已关闭，数据传输结束。
        // 如果 iret < 0，则发生错误。常见错误可能是网络问题或对方异常断开连接。
        if ((iret = recv(clientfd, buffer, sizeof(buffer), 0)) <= 0)
        {
            cout << "iret=" << iret << endl;
            break;
        }
        cout << "接收：" << buffer << endl;

        strcpy(buffer, "ok"); // 生成回应报文内容。
        // 向客户端发送回应报文。
        if ((iret = send(clientfd, buffer, strlen(buffer), 0)) <= 0)
        {
            perror("send");
            break;
        }
        cout << "发送：" << buffer << endl;
    }

    // 第6步：关闭socket，释放资源。
    close(listenfd); // 关闭服务端用于监听的socket。
    close(clientfd); // 关闭客户端连上来的socket。
}
```


# I/O复用模型，selcet，poll，epoll，非阻塞的IO

### select
- client.cpp
```cpp
// 网络通讯的客户端程序。
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <time.h>

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        printf("usage:./client ip port\n"); return -1;
    }

    int sockfd;
    struct sockaddr_in servaddr;
    char buf[1024];
 
    if ((sockfd=socket(AF_INET,SOCK_STREAM,0))<0) { printf("socket() failed.\n"); return -1; }
	
    memset(&servaddr,0,sizeof(servaddr));
    servaddr.sin_family=AF_INET;
    servaddr.sin_port=htons(atoi(argv[2]));
    servaddr.sin_addr.s_addr=inet_addr(argv[1]);

    if (connect(sockfd, (struct sockaddr *)&servaddr,sizeof(servaddr)) != 0)
    {
        printf("connect(%s:%s) failed.\n",argv[1],argv[2]); close(sockfd);  return -1;
    }

    printf("connect ok.\n");

    // printf("开始时间：%d",time(0));

    for (int ii=0;ii<200000;ii++)
    {
        // 从命令行输入内容。
        memset(buf,0,sizeof(buf));
        printf("please input:"); scanf("%s",buf);

        if (send(sockfd,buf,strlen(buf),0) <=0)
        { 
            printf("write() failed.\n");  close(sockfd);  return -1;
        }
		
        memset(buf,0,sizeof(buf));
        if (recv(sockfd,buf,sizeof(buf),0) <=0) 
        { 
            printf("read() failed.\n");  close(sockfd);  return -1;
        }

        printf("recv:%s\n",buf);
    }

    // printf("结束时间：%d",time(0));
} 
```

- tcpselect.cpp
```cpp
/*
 * 程序名：tcpselect.cpp，此程序用于演示采用select模型实现网络通讯的服务端。
 */
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <sys/fcntl.h>

// 初始化服务端的监听端口。
int initserver(int port);

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        printf("usage: ./tcpselect port\n");
        return -1;
    }

    // 初始化服务端用于监听的socket。
    int listensock = initserver(atoi(argv[1]));
    printf("listensock=%d\n", listensock);

    if (listensock < 0)
    {
        printf("initserver() failed.\n");
        return -1;
    }

    // 读事件：1）已连接队列中有已经准备好的socket（有新的客户端连上来了）；
    //               2）接收缓存中有数据可以读（对端发送的报文已到达）；
    //               3）tcp连接已断开（对端调用close()函数关闭了连接）。
    // 写事件：发送缓冲区没有满，可以写入数据（可以向对端发送报文）。

    // C语言提供了4个宏来操控位图
    // void FD_CLR(int fd, fd_set * set); // 把socket从bitmap中删除
    // int FD_ISSET(int fd, fd_set * set); // 判断socket是否在bitmap中，如果不在返回0，在则返回大于0
    // void FD_SET(int fd, fd_set * set); // 把socket加入到集合中 fd是socket set是bitmap的地址
    // void FD_ZERO(fd_set * set); // 初始化位图，把全部的1024个位置全部变为0
    // fd_set 是int[32]的数组，里面占用bit为 4*8*32=1024位，是一个bitmap
    fd_set readfds;               // 需要监视读事件的socket的集合，大小为16字节（1024位）的bitmap。
    FD_ZERO(&readfds);            // 初始化readfds，把bitmap的每一位都置为0。
    FD_SET(listensock, &readfds); // 把服务端用于监听的socket加入readfds。

    int maxfd = listensock; // readfds中socket的最大值。

    // select有超时机制，如果在timeout时间后，如果监视的socket没有任何事件发生，就会返回超时
    while (true) // 事件循环。
    {
        // 用于表示超时时间的结构体。
        struct timeval timeout;
        timeout.tv_sec = 10; // 秒
        timeout.tv_usec = 0; // 微秒。

        fd_set tmpfds = readfds; // 在select()函数中，会修改bitmap，所以，要把readfds复制一份给tmpfds，再把tmpfds传给select()。

        // 调用select() 等待事件的发生（监视哪些socket发生了事件)。
        // maxfd+1:告诉select，它的bitmap有多大
        // 在select()函数中，会修改bitmap，所以，要把readfds复制一份给tmpfds，再把tmpfds传给select()
        // 第三个参数是需要监视的写事件的bitmap，如果不关心写事件，则填空
        // 第四个参数填需要关心的异常事件的bitmap，在监视普通IO事件的时候可以用到第四个参数，网络编程中用不到
        // 第五个参数填超时时间，如果没有超时时间，NULL表示永远等待
        int infds = select(maxfd + 1, &tmpfds, NULL, NULL, 0);
        // int infds = select(maxfd + 1, &tmpfds, NULL, NULL, &timeout);

        // 如果infds<0，表示调用select()失败。会设置errno全局变量
        if (infds < 0)
        {
            perror("select() failed");
            break;
        }

        // 如果infds==0，表示select()超时。
        if (infds == 0)
        {
            printf("select() timeout.\n");
            continue;
        }

        // 如果infds>0，表示有事件发生，infds存放了已发生事件的个数。
        for (int eventfd = 0; eventfd <= maxfd; eventfd++)
        {
            if (FD_ISSET(eventfd, &tmpfds) == 0)
                continue; // 如果eventfd在bitmap中的标志为0，表示它没有事件，continue

            // 如果发生事件的是listensock，表示已连接队列中有已经准备好的socket（有新的客户端连上来了）。
            if (eventfd == listensock)
            {
                struct sockaddr_in client;
                socklen_t len = sizeof(client);
                int clientsock = accept(listensock, (struct sockaddr *)&client, &len);
                if (clientsock < 0)
                {
                    perror("accept() failed");
                    continue;
                }

                printf("accept client(socket=%d) ok.\n", clientsock);

                FD_SET(clientsock, &readfds); // 把bitmap中新连上来的客户端的标志位置为1。

                if (maxfd < clientsock)
                    maxfd = clientsock; // 更新maxfd的值。
            }
            else
            {
                // 如果是客户端连接的socke有事件，表示接收缓存中有数据可以读（对端发送的报文已到达），或者有客户端已断开连接。
                char buffer[1024]; // 存放从接收缓冲区中读取的数据。
                memset(buffer, 0, sizeof(buffer));
                // recv<0：表示已经发生了错误
                // recv=0：连接已经被对方断开
                // recv>0：表示成功读取的字节数
                if (recv(eventfd, buffer, sizeof(buffer), 0) <= 0)
                {
                    // 如果客户端的连接已断开。
                    printf("client(eventfd=%d) disconnected.\n", eventfd);

                    close(eventfd); // 关闭客户端的socket

                    FD_CLR(eventfd, &readfds); // 把bitmap中已关闭客户端的标志位清空。

                    if (eventfd == maxfd) // 重新计算maxfd的值，注意，只有当eventfd==maxfd时才需要计算。
                    {
                        for (int ii = maxfd; ii > 0; ii--) // 从后面往前找。
                        {
                            if (FD_ISSET(ii, &readfds))
                            {
                                maxfd = ii;
                                break;
                            }
                        }
                    }
                }
                else
                {
                    // 如果客户端有报文发过来。
                    printf("recv(eventfd=%d):%s\n", eventfd, buffer);

                    // 把接收到的报文内容原封不动的发回去。
                    send(eventfd, buffer, strlen(buffer), 0);
                }
            }
        }
    }

    return 0;
}

// 初始化服务端的监听端口。
int initserver(int port)
{
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0)
    {
        perror("socket() failed");
        return -1;
    }

    int opt = 1;
    unsigned int len = sizeof(opt);
    setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &opt, len);

    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(port);

    if (bind(sock, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
    {
        perror("bind() failed");
        close(sock);
        return -1;
    }

    if (listen(sock, 5) != 0)
    {
        perror("listen() failed");
        close(sock);
        return -1;
    }

    return sock;
}

```

### poll模型
- tcppoll.cpp
```cpp
/*
 * 程序名：tcppoll.cpp，此程序用于演示采用poll模型实现网络通讯的服务端。
 * 作者：吴从周
 */
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <poll.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <sys/fcntl.h>

// 初始化服务端的监听端口。
int initserver(int port);

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        printf("usage: ./tcppoll port\n");
        return -1;
    }

    // 初始化服务端用于监听的socket。
    int listensock = initserver(atoi(argv[1]));
    printf("listensock=%d\n", listensock);

    if (listensock < 0)
    {
        printf("initserver() failed.\n");
        return -1;
    }

    // 在select模型中用bitmap存放需要监视的socket
    // 在poll模型中用结构体数组存放需要监视的socket
    // struct pollfd{
    //     int fd; // 需要监视的socket
    //     short events; // 需要监视的事件
    //     short revents; // poll返回的事件
    // };
    // 在程序中我们设置需要监视的socket和事件，把结构体传给poll
    // 如果有事件发生了，poll只会修改这个成员(revents)，另外两个成员（fd和events）不会修改
    // 在select模型中，bitmap模型大小固定是1024，而poll模型中，数组的大小由我们决定
    pollfd fds[2048]; // fds存放需要监视的socket。

    // 初始化数组，把全部的socket设置为-1，如果数组中的socket的值为-1，那么，poll将忽略它。
    for (int ii = 0; ii < 2048; ii++)
        fds[ii].fd = -1;

    // 打算让poll监视listensock读事件。
    fds[listensock].fd = listensock;
    fds[listensock].events = POLLIN; // POLLIN表示读事件，POLLOUT表示写事件。
    // fds[listensock].events=POLLIN|POLLOUT; // 既监视读事件，也监视写事件

    int maxfd = listensock; // fds数组中需要监视的socket的实际大小。
    //  0  1  2  3  4  5  6 ... 2047  结构体数组位置下标
    // -1 -1 -1  3  4 -1  6 ... -1    按数组索引填写，一般选择这个，效率高，写代码方便
    //  3  4  6 -1 -1 -1 -1 ... -1    按空格位置补齐，数组空间利用率高

    while (true) // 事件循环。
    {
        // 调用poll() 等待事件的发生（监视哪些socket发生了事件)。
        // 第一个参数：结构体数组名
        // 第二个参数：最大的socketId+1，表示有效的socket的数量
        // 第三个参数：超时时间，ms
        int infds = poll(fds, maxfd + 1, 10000); // 超时时间为10秒。

        // 如果infds<0，表示调用poll()失败。
        if (infds < 0)
        {
            perror("poll() failed");
            break;
        }

        // 如果infds==0，表示poll()超时。
        if (infds == 0)
        {
            printf("poll() timeout.\n");
            continue;
        }

        // 如果infds>0，表示有事件发生，infds存放了已发生事件的个数。
        for (int eventfd = 0; eventfd <= maxfd; eventfd++)
        {
            if (fds[eventfd].fd < 0)
                continue; // 如果fd为负，忽略它。

            if ((fds[eventfd].revents & POLLIN) == 0)
                continue; // 如果没有读事件，continue

            // 如果发生事件的是listensock，表示已连接队列中有已经准备好的socket（有新的客户端连上来了）。
            if (eventfd == listensock)
            {
                struct sockaddr_in client;
                socklen_t len = sizeof(client);
                int clientsock = accept(listensock, (struct sockaddr *)&client, &len);
                if (clientsock < 0)
                {
                    perror("accept() failed");
                    continue;
                }

                printf("accept client(socket=%d) ok.\n", clientsock);

                // 修改fds数组中clientsock位置的元素
                // 表示监听clientsock的事件
                fds[clientsock].fd = clientsock;
                fds[clientsock].events = POLLIN;

                if (maxfd < clientsock)
                    maxfd = clientsock; // 更新maxfd的值。
            }
            else
            {
                // 如果是客户端连接的socke有事件，表示有报文发过来了或者连接已断开。

                char buffer[1024]; // 存放从客户端读取的数据。
                memset(buffer, 0, sizeof(buffer));
                if (recv(eventfd, buffer, sizeof(buffer), 0) <= 0)
                {
                    // 如果客户端的连接已断开。
                    printf("client(eventfd=%d) disconnected.\n", eventfd);

                    close(eventfd);       // 关闭客户端的socket。
                    fds[eventfd].fd = -1; // 修改fds数组中clientsock位置的元素，置为-1，poll将忽略该元素。

                    // 重新计算maxfd的值，注意，只有当eventfd==maxfd时才需要计算。
                    if (eventfd == maxfd)
                    {
                        for (int ii = maxfd; ii > 0; ii--) // 从后面往前找。
                        {
                            if (fds[ii].fd != -1)
                            {
                                maxfd = ii;
                                break;
                            }
                        }
                    }
                }
                else
                {
                    // 如果客户端有报文发过来。
                    printf("recv(eventfd=%d):%s\n", eventfd, buffer);

                    send(eventfd, buffer, strlen(buffer), 0);
                }
            }
        }
    }

    return 0;
}

// 初始化服务端的监听端口。
int initserver(int port)
{
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0)
    {
        perror("socket() failed");
        return -1;
    }

    int opt = 1;
    unsigned int len = sizeof(opt);
    setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &opt, len);

    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(port);

    if (bind(sock, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
    {
        perror("bind() failed");
        close(sock);
        return -1;
    }

    if (listen(sock, 5) != 0)
    {
        perror("listen() failed");
        close(sock);
        return -1;
    }

    return sock;
}
```


### epoll模型
- tcpepoll.cpp
```cpp
/*
 * 程序名：tcpepoll.cpp，此程序用于演示采用epoll模型实现网络通讯的服务端。
 * 作者：吴从周
 */
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <arpa/inet.h>
#include <sys/fcntl.h>
#include <sys/epoll.h>

// 初始化服务端的监听端口。
int initserver(int port);

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        printf("usage: ./tcpepoll port\n");
        return -1;
    }

    // 初始化服务端用于监听的socket。
    int listensock = initserver(atoi(argv[1]));
    printf("listensock=%d\n", listensock);

    if (listensock < 0)
    {
        printf("initserver() failed.\n");
        return -1;
    }

    // 创建epoll句柄
    // 创建一个epoll实例，类似于文件描述符，是一个整数
    // 参数随便，大于0就行
    int epollfd = epoll_create(1);

    // 为服务端的listensock准备读事件
    // typedef union epoll_data{
    //     void *ptr;
    //     int fd;
    //     uint32_t u32;
    //     uint64_t u64;
    // }epoll_data_t;
    // struct epoll_event{
    //     uint32_t events; // epoll事件 EPOLLIN读事件，EPOLLOUT写事件
    //     epoll_data_t data; // 用户数据变量
    // };
    epoll_event ev;          // 声明事件的数据结构。
    ev.data.fd = listensock; // 指定事件的自定义数据，会随着epoll_wait()返回的事件一并返回。
    // 如果启用下面的代码，会导致data共同体中的fd成员失效，后续中的socket判断会用到这个，但是不能用
    // 因为是水平触发，所以会一直触发
    // ev.data.ptr=(void*)"超女";   // 指定事件的自定义数据，会随着epoll_wait()返回的事件一并返回。
    ev.events = EPOLLIN; // 打算让epoll监视listensock的读事件。

    // 第一个参数是epollfd句柄
    // 第二个参数是EPOLL_CTL_ADD宏
    // EPOLL_CTL_ADD：向 epoll 实例中添加一个文件描述符，并设置对该描述符的监听事件。
    // EPOLL_CTL_MOD：修改 epoll 实例中已经存在的文件描述符的监听事件。
    // EPOLL_CTL_DEL：从 epoll 实例中删除文件描述符，不再监听它的事件。
    // 第三个参数是需要监听的socket
    // 第四个参数是事件结构体的地址
    // 将 listensock 添加（EPOLL_CTL_ADD）到 epollfd 指向的 epoll 实例中，并使用 ev 中的事件信息作为关注的事件（例如 EPOLLIN）。
    epoll_ctl(epollfd, EPOLL_CTL_ADD, listensock, &ev); // 把需要监视的socket和事件加入epollfd中。

    // 自己定义的数组，大一点，小一点都没事
    epoll_event evs[10]; // 存放epoll返回的事件。

    while (true) // 事件循环。
    {
        // 等待监视的socket有事件发生。
        // 第一个参数：epoll句柄，epoll实例
        // 第二个参数：如果epoll监听的socket有事件发生，将发生事件放在evs数组中
        // indfs是返回值，表示有事件发生的socket的数量
        int infds = epoll_wait(epollfd, evs, 10, -1);

        // 返回失败。
        if (infds < 0)
        {
            perror("epoll() failed");
            break;
        }

        // 超时。
        if (infds == 0)
        {
            printf("epoll() timeout.\n");
            continue;
        }

        // 如果infds>0，表示有事件发生的socket的数量。
        for (int ii = 0; ii < infds; ii++) // 遍历epoll返回的数组evs。
        {
            // printf("ptr=%s,events=%d\n",evs[ii].data.ptr,evs[ii].events);

            // 如果发生事件的是listensock，表示有新的客户端连上来。
            if (evs[ii].data.fd == listensock)
            {
                struct sockaddr_in client;
                socklen_t len = sizeof(client);
                int clientsock = accept(listensock, (struct sockaddr *)&client, &len);

                printf("accept client(socket=%d) ok.\n", clientsock);

                // 为新客户端准备读事件，并添加到epoll中。
                ev.data.fd = clientsock;
                ev.events = EPOLLIN;
                epoll_ctl(epollfd, EPOLL_CTL_ADD, clientsock, &ev);
            }
            else
            {
                // 如果是客户端连接的socket有事件，表示有报文发过来或者连接已断开。
                char buffer[1024]; // 存放从客户端读取的数据。
                memset(buffer, 0, sizeof(buffer));
                // evs[ii].data.fd存放了socket的值
                // buffer是客户端的数据
                if (recv(evs[ii].data.fd, buffer, sizeof(buffer), 0) <= 0)
                {
                    // 如果客户端的连接已断开。
                    printf("client(eventfd=%d) disconnected.\n", evs[ii].data.fd);
                    close(evs[ii].data.fd); // 关闭客户端的socket
                    // 从epollfd中删除客户端的socket，如果socket被关闭了，会自动从epollfd中删除，所以，以下代码不必启用。
                    // epoll_ctl(epollfd,EPOLL_CTL_DEL,evs[ii].data.fd,0);
                }
                else
                {
                    // 如果客户端有报文发过来。
                    printf("recv(eventfd=%d):%s\n", evs[ii].data.fd, buffer);

                    // 把接收到的报文内容原封不动的发回去。
                    send(evs[ii].data.fd, buffer, strlen(buffer), 0);
                }
            }
        }
    }

    return 0;
}

// 初始化服务端的监听端口。
int initserver(int port)
{
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0)
    {
        perror("socket() failed");
        return -1;
    }

    int opt = 1;
    unsigned int len = sizeof(opt);
    setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &opt, len);

    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(port);

    if (bind(sock, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
    {
        perror("bind() failed");
        close(sock);
        return -1;
    }

    if (listen(sock, 5) != 0)
    {
        perror("listen() failed");
        close(sock);
        return -1;
    }

    return sock;
}
```

### 阻塞与非阻塞IO
> 阻塞：在进程/线程中，发起一个调用，在调用返回之前，进程/线程会被阻塞等待，等待中的进程/线程会让出CPU的使用权
> 非阻塞：在进程/线程中，发起一个调用，会立即返回
> 会阻塞的四个函数：connect(),accept(),send(),recv()
> 在传统的网络服务端程序中（每连接，每个线程/进程），采用阻塞IO
> 在IO复用的模型中，事件循环不能被阻塞在任何环节，所以，应该采用非阻塞IO

#### 非阻塞IO-connect()
- 对非阻塞的IO调用connect()函数，不管是否能够连接成功，connect()都会立即返回失败，`errno==EINPROGRESS`
对非阻塞的IO调用connect()函数后，如果socket的状态是可写的，证明连接是成功的，否则是失败的

#### 非阻塞IO-accept()
- 对非阻塞的IO调用accept()，如果已连接队列中没有socket,函数立即返回失败，`errno==EAGAIN`

#### 非阻塞IO-recv()
- 对非阻塞的IO调用recv(),如果没有数据可读(接收缓冲区为空),函数立即返回失败,`errno==EAGAIN`

#### 非阻塞IO-send()
- 对非阻塞的IO调用send(),如果socket不可写（发送缓冲区已经满），函数立即返回失败，`errno==EAGAIN`

- client1.cpp
```cpp
// 网络通讯的客户端程序。
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <poll.h>
#include <fcntl.h>

// 把socket设置成非阻塞。
int setnonblocking(int fd)
{
    int flags;

    // 获取fd的状态。
    if ((flags = fcntl(fd, F_GETFL, 0)) == -1)
        flags = 0;
    // 在原来的状态上设置socket为非阻塞
    return fcntl(fd, F_SETFL, flags | O_NONBLOCK);
}

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        printf("usage:./client1 ip port\n");
        return -1;
    }

    int sockfd;
    struct sockaddr_in servaddr;
    char buf[1024];

    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0)
    {
        printf("socket() failed.\n");
        return -1;
    }

    setnonblocking(sockfd); // 把sockfd设置成非阻塞。

    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(atoi(argv[2]));
    servaddr.sin_addr.s_addr = inet_addr(argv[1]);

    // 因为connect在客户端使用的多，在服务端用的少，我们需要处理的是服务端的非阻塞
    // 作为了解即可
    if (connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) != 0)
    {
        // 对于非阻塞的connect(),调用connect()都是返回EINPROGRESS
        if (errno != EINPROGRESS)
        {
            printf("connect(%s:%s) failed.\n", argv[1], argv[2]);
            close(sockfd);
            return -1;
        }
    }

    // 这里是判断sockfd是否是可写的，如果是可写的
    // 表示连接是成功的
    pollfd fds;
    fds.fd = sockfd;
    fds.events = POLLOUT;
    // 用poll来监听sockfd的写事件
    poll(&fds, 1, -1);
    // 如果是可写的，那么表示连接成功
    if (fds.revents == POLLOUT)
        printf("connect ok.\n");
    else
        printf("connect failed.\n");

    return 0;
    // printf("开始时间：%d",time(0));

    for (int ii = 0; ii < 200000; ii++)
    {
        // 从命令行输入内容。
        memset(buf, 0, sizeof(buf));
        printf("please input:");
        scanf("%s", buf);
        // strcpy(buf,"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbccccccccccccccccddddddddddddd");

        if (send(sockfd, buf, strlen(buf), 0) <= 0)
        {
            printf("write() failed.\n");
            close(sockfd);
            return -1;
        }

        memset(buf, 0, sizeof(buf));
        if (recv(sockfd, buf, sizeof(buf), 0) <= 0)
        {
            printf("read() failed.\n");
            close(sockfd);
            return -1;
        }

        printf("recv:%s\n", buf);
    }

    // printf("结束时间：%d",time(0));
}

```

- tcpepoll1.cpp
```cpp
/*
 * 程序名：tcpepoll1.cpp，此程序用于临时的演示。
 * 作者：吴从周
 */
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <arpa/inet.h>
#include <sys/fcntl.h>
#include <sys/epoll.h>

// 把socket设置成非阻塞。
int setnonblocking(int fd)
{
    int flags;

    // 获取fd（socket）的状态。
    if ((flags = fcntl(fd, F_GETFL, 0)) == -1)
        flags = 0;

    // 在原来的状态上设置socket为非阻塞
    return fcntl(fd, F_SETFL, flags | O_NONBLOCK);
}

// 初始化服务端的监听端口。
int initserver(int port);

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        printf("usage: ./tcpepoll1 port\n");
        return -1;
    }

    // 初始化服务端用于监听的socket。
    int listensock = initserver(atoi(argv[1]));
    printf("listensock=%d\n", listensock);

    setnonblocking(listensock); // 把监听的socket设置为非阻塞。

    while (true)
    {
        if (accept(listensock, 0, 0) == -1)
        {
            // 非阻塞都会产生EAGAIN错误，如果不是这个错误，才真的是出错了
            if (errno != EAGAIN)
            {
                perror("accept:");
                return -1;
            }
        }
        else
            break;
    }

    printf("客户端已连接。\n");

    return 0;

    if (listensock < 0)
    {
        printf("initserver() failed.\n");
        return -1;
    }

    // 创建epoll句柄。
    int epollfd = epoll_create(1);

    // 为服务端的listensock准备读事件。
    epoll_event ev;          // 声明事件的数据结构。
    ev.data.fd = listensock; // 指定事件的自定义数据，会随着epoll_wait()返回的事件一并返回。
    // ev.data.ptr=(void*)"超女";   // 指定事件的自定义数据，会随着epoll_wait()返回的事件一并返回。
    ev.events = EPOLLIN; // 打算让epoll监视listensock的读事件。

    epoll_ctl(epollfd, EPOLL_CTL_ADD, listensock, &ev); // 把需要监视的socket和事件加入epollfd中。

    epoll_event evs[10]; // 存放epoll返回的事件。

    while (true) // 事件循环。
    {
        // 等待监视的socket有事件发生。
        int infds = epoll_wait(epollfd, evs, 10, -1);

        // 返回失败。
        if (infds < 0)
        {
            perror("epoll() failed");
            break;
        }

        // 超时。
        if (infds == 0)
        {
            printf("epoll() timeout.\n");
            continue;
        }

        // 如果infds>0，表示有事件发生的socket的数量。
        for (int ii = 0; ii < infds; ii++) // 遍历epoll返回的数组evs。
        {
            // printf("ptr=%s,events=%d\n",evs[ii].data.ptr,evs[ii].events);

            // 如果发生事件的是listensock，表示有新的客户端连上来。
            if (evs[ii].data.fd == listensock)
            {
                struct sockaddr_in client;
                socklen_t len = sizeof(client);
                int clientsock = accept(listensock, (struct sockaddr *)&client, &len);

                printf("accept client(socket=%d) ok.\n", clientsock);

                // 为新客户端准备读事件，并添加到epoll中。
                ev.data.fd = clientsock;
                ev.events = EPOLLIN;
                epoll_ctl(epollfd, EPOLL_CTL_ADD, clientsock, &ev);
            }
            else
            {
                // 如果是客户端连接的socke有事件，表示有报文发过来或者连接已断开。
                char buffer[1024]; // 存放从客户端读取的数据。
                memset(buffer, 0, sizeof(buffer));
                if (recv(evs[ii].data.fd, buffer, sizeof(buffer), 0) <= 0)
                {
                    // 如果客户端的连接已断开。
                    printf("client(eventfd=%d) disconnected.\n", evs[ii].data.fd);
                    close(evs[ii].data.fd); // 关闭客户端的socket
                    // 从epollfd中删除客户端的socket，如果socket被关闭了，会自动从epollfd中删除，所以，以下代码不必启用。
                    // epoll_ctl(epollfd,EPOLL_CTL_DEL,evs[ii].data.fd,0);
                }
                else
                {
                    // 如果客户端有报文发过来。
                    printf("recv(eventfd=%d):%s\n", evs[ii].data.fd, buffer);

                    // 把接收到的报文内容原封不动的发回去。
                    send(evs[ii].data.fd, buffer, strlen(buffer), 0);
                }
            }
        }
    }

    return 0;
}

// 初始化服务端的监听端口。
int initserver(int port)
{
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0)
    {
        perror("socket() failed");
        return -1;
    }

    int opt = 1;
    unsigned int len = sizeof(opt);
    setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &opt, len);

    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(port);

    if (bind(sock, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
    {
        perror("bind() failed");
        close(sock);
        return -1;
    }

    if (listen(sock, 5) != 0)
    {
        perror("listen() failed");
        close(sock);
        return -1;
    }

    return sock;
}
```

### 水平触发(Level-triggered,LT)和边缘触发(Edge-triggered,ET)
- **水平触发 (Level-triggered)**：即事件持续有效，只要条件满足，就会持续通知。
- **边缘触发 (Edge-triggered)**：即事件在状态发生变化的一瞬间触发，比如从低到高或从高到低的变化。
- client2.cpp
```cpp
// 网络通讯的客户端程序。
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <time.h>

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        printf("usage:./client2 ip port\n"); return -1;
    }

    int sockfd;
    struct sockaddr_in servaddr;
    char buf[1024];
 
    if ((sockfd=socket(AF_INET,SOCK_STREAM,0))<0) { printf("socket() failed.\n"); return -1; }
	
    memset(&servaddr,0,sizeof(servaddr));
    servaddr.sin_family=AF_INET;
    servaddr.sin_port=htons(atoi(argv[2]));
    servaddr.sin_addr.s_addr=inet_addr(argv[1]);

    if (connect(sockfd, (struct sockaddr *)&servaddr,sizeof(servaddr)) != 0)
    {
        printf("connect(%s:%s) failed.\n",argv[1],argv[2]); close(sockfd);  return -1;
    }

    printf("connect ok.\n");

    // printf("开始时间：%d",time(0));

    for (int ii=0;ii<200000;ii++)
    {
        // 从命令行输入内容。
        memset(buf,0,sizeof(buf));
        printf("please input:"); scanf("%s",buf);
        // strcpy(buf,"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbccccccccccccccccddddddddddddd");

        if (send(sockfd,buf,strlen(buf),0) <=0)
        { 
            printf("write() failed.\n");  close(sockfd);  return -1;
        }
		
        memset(buf,0,sizeof(buf));
        if (recv(sockfd,buf,sizeof(buf),0) <=0) 
        { 
            printf("read() failed.\n");  close(sockfd);  return -1;
        }

        printf("recv:%s\n",buf);
    }

    // printf("结束时间：%d",time(0));
} 
```

- tcpepoll2.cpp
```cpp
/*
 * 程序名：tcpepoll2.cpp，此程序用于演示采用epoll模型的边缘触发。
 * 作者：吴从周
 */
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <arpa/inet.h>
#include <sys/fcntl.h>
#include <sys/epoll.h>

// 把socket设置成非阻塞。
int setnonblocking(int fd)
{
    int flags;

    // 获取fd的状态。
    if ((flags = fcntl(fd, F_GETFL, 0)) == -1)
        flags = 0;

    return fcntl(fd, F_SETFL, flags | O_NONBLOCK);
}

// 初始化服务端的监听端口。
int initserver(int port);

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        printf("usage: ./tcpepoll2 port\n");
        return -1;
    }

    // 初始化服务端用于监听的socket。
    int listensock = initserver(atoi(argv[1]));
    printf("listensock=%d\n", listensock);

    if (listensock < 0)
    {
        printf("initserver() failed.\n");
        return -1;
    }

    setnonblocking(listensock); // 把listensock设置为非阻塞。

    // 创建epoll句柄。
    int epollfd = epoll_create(1);

    // 为服务端的listensock准备读事件。
    epoll_event ev;          // 声明事件的数据结构。
    ev.data.fd = listensock; // 指定事件的自定义数据，会随着epoll_wait()返回的事件一并返回。
    // ev.data.ptr=(void*)"超女";   // 指定事件的自定义数据，会随着epoll_wait()返回的事件一并返回。
    ev.events = EPOLLIN; // 打算让epoll监视listensock的读事件，LT（水平）模式。
    // ev.events=EPOLLIN | EPOLLET;      // 打算让epoll监视listensock的读事件，ET（边缘）模式。

    epoll_ctl(epollfd, EPOLL_CTL_ADD, listensock, &ev); // 把需要监视的socket和事件加入epollfd中。

    epoll_event evs[10]; // 存放epoll返回的事件。

    while (true) // 事件循环。
    {
        // 等待监视的socket有事件发生。
        int infds = epoll_wait(epollfd, evs, 10, -1);

        // 返回失败。
        if (infds < 0)
        {
            perror("epoll() failed");
            break;
        }

        // 超时。
        if (infds == 0)
        {
            printf("epoll() timeout.\n");
            continue;
        }

        // 如果infds>0，表示有事件发生的socket的数量。
        for (int ii = 0; ii < infds; ii++) // 遍历epoll返回的数组evs。
        {
            // printf("ptr=%s,events=%d\n",evs[ii].data.ptr,evs[ii].events);

            // 如果发生事件的是listensock，表示有新的客户端连上来。
            if (evs[ii].data.fd == listensock)
            {
                // ET(边缘触发)需要用非阻塞的循环来写
                // 因为边缘触发可能会有socket未处理，但也不会再次通知
                // 只能通过循环accept()来一直找是否还有未连接的socket
                // 必须要用非阻塞的，否则，没有connect的时候会阻塞在accept()这里
                while (true)
                {
                    struct sockaddr_in client;
                    socklen_t len = sizeof(client);
                    int clientsock = accept(listensock, (struct sockaddr *)&client, &len);
                    // clientsock < 0：accept 返回值 clientsock 小于 0 时表示发生了错误，未能成功接受连接
                    // errno == EAGAIN：在非阻塞模式下，如果没有新的连接可以接受，accept 会返回 -1，并将 errno 设置为 EAGAIN，表示当前没有可用的客户端连接
                    // 表示已连接队列中没有socket了
                    if ((clientsock < 0) && (errno == EAGAIN))
                        break;

                    printf("accept client(socket=%d) ok.\n", clientsock);

                    // 为新客户端准备读事件，并添加到epoll中。
                    setnonblocking(clientsock); // 把客户端连接的socket设置为非阻塞。
                    ev.data.fd = clientsock;
                    // ev.events=EPOLLOUT;                       // LT-水平触发。
                    ev.events = EPOLLOUT | EPOLLET; // ET-边缘触发。
                    epoll_ctl(epollfd, EPOLL_CTL_ADD, clientsock, &ev);
                }
            }
            else
            {
                printf("触发了写件事。\n");
                // 这里是一直发送数据，会存在发送缓冲区，而客户端读取速度慢一点
                // 服务端的发送缓冲区就会满，后续就等待客户端的接收缓冲区接收数据，发送缓冲区就会空出位置
                // 发送缓冲区就从满->不满，此时就会在边缘模式下再次触发写事件
                for (int ii = 0; ii < 10000000; ii++)
                {
                    if (send(ev.data.fd, "aaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbb", 30, 0) <= 0)
                    {
                        if (errno == EAGAIN)
                        {
                            printf("发送缓冲区已填懣。\n");
                            break;
                        }
                    }
                }
                /* 读事件代码
                // 如果是客户端连接的socke有事件，表示有报文发过来或者连接已断开。
                // 边缘触发的代码可以用于水平触发，而水平触发的代码不能用于边缘触发
                // 修改服务器的监听/客户端连接的socket的events为水平或者边缘触发
                char buffer[1024];       // 存放从客户端读取的数据。
                memset(buffer,0,sizeof(buffer));
                int    readn;                 // 每次调用recv()的返回值。
                char *ptr=buffer;        // buffer的位置指针。
                while (true)
                {
                    // readn是读取的字节数，如果大于0，表示还有数据，继续读取，ptr是当前buffer中可以存放数据的位置
                    if ( (readn=recv(evs[ii].data.fd,ptr,5,0))<=0 )
                    {
                        if ( (readn<0) && (errno==EAGAIN) )
                        {   // 如果数据被读取完了，把接收到的报文内容原封不动的发回去。
                            send(evs[ii].data.fd,buffer,strlen(buffer),0);
                            printf("recv(eventfd=%d):%s\n",evs[ii].data.fd,buffer);
                        }
                        else
                        {
                            // 如果客户端的连接已断开。
                            printf("client(eventfd=%d) disconnected.\n",evs[ii].data.fd);
                            close(evs[ii].data.fd);            // 关闭客户端的socket
                        }

                        break;        // 跳出循环。
                    }
                    else
                        ptr=ptr+readn;                    // buffer的位置指针后移。
                }
                */
            }
        }
    }

    return 0;
}

// 初始化服务端的监听端口。
int initserver(int port)
{
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0)
    {
        perror("socket() failed");
        return -1;
    }

    int opt = 1;
    unsigned int len = sizeof(opt);
    setsockopt(sock, SOL_SOCKET, SO_REUSEADDR, &opt, len);

    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(port);

    if (bind(sock, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0)
    {
        perror("bind() failed");
        close(sock);
        return -1;
    }

    if (listen(sock, 5) != 0)
    {
        perror("listen() failed");
        close(sock);
        return -1;
    }

    return sock;
}
```

