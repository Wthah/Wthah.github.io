---
title: Netty
date: 2022/7/5 11:21:25
tags:
  - Netty
  - 网络
categories:
  - 网络
cover: /img/netty/netty.jpg
---

# Netty

## 简介

> **Netty**是一个java开源框架。Netty提供异步的、事件驱动的网络应用程序框架和工具，用以快速开发高性能、高可靠性的网络服务器和客户端程序。
>
> Netty是一个NIO客户端、服务端框架。允许快速简单的开发网络应用程序。例如：服务端和客户端之间的协议。它最牛逼的地方在于简化了网络编程规范。例如:TCP和UDP的Socket服务

## 安装

```xml
<!-- https://mvnrepository.com/artifact/io.netty/netty-all -->
<dependency>
    <groupId>io.netty</groupId>
    <artifactId>netty-all</artifactId>
    <version>4.1.78.Final</version>
</dependency>
```

## 编写服务端

```java
package cn.sxt.netty;
 
import io.netty.buffer.ByteBuf;
 
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
 
/**
 * 处理服务器端通道
 */
public class DiscardServerHandler extends ChannelInboundHandlerAdapter { // (1)
 
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) { // (2)
        // 以静默方式丢弃接收的数据
        try{
            
        }finally{
	        ((ByteBuf) msg).release(); // (3)
        }
    }
 
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) { // (4)
        // 出现异常时关闭连接。
        cause.printStackTrace();
        ctx.close();
    }
}
```

> （1）`ChannelInboundHandlerAdapter`是`ChannelInboundHandler`的一个实现，负责覆盖各种事件处置程序方法
>
> （2）`ChannelRead()`用于处理程序方法，每当客户端收到新数据时，使用该方法来接受客户端的消息，在此示例中，接收到的消息的类型为`ByteBuf`
>
> （3）`ByteBuf`是引用计数的对象，必须通过`release()`方法显式释放

###### ByteBuf学习

> https://blog.csdn.net/usagoole/article/details/88024517?ops_request_misc=&request_id=&biz_id=102&utm_term=ByteBuf&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-88024517.142^v31^pc_rank_34,185^v2^tag_show&spm=1018.2226.3001.4187

## 调用Handler

```java
package cn.sxt.netty;
 
import io.netty.bootstrap.ServerBootstrap;
 
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelOption;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
 
/**
 * Discards any incoming data.
 */
public class DiscardServer {
 
    private int port;
 
    public DiscardServer(int port) {
        this.port = port;
    }
 
    public void run() throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup(); // (1)
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try {
            ServerBootstrap b = new ServerBootstrap(); // (2)
            b.group(bossGroup, workerGroup)
             .channel(NioServerSocketChannel.class) // (3)
             .childHandler(new ChannelInitializer<SocketChannel>() { // (4)
                 @Override
                 public void initChannel(SocketChannel ch) throws Exception {
                     ch.pipeline().addLast(new DiscardServerHandler());
                 }
             })
             .option(ChannelOption.SO_BACKLOG, 128)          // (5)
             .childOption(ChannelOption.SO_KEEPALIVE, true); // (6)
 
            // Bind and start to accept incoming connections.
            ChannelFuture f = b.bind(port).sync(); // (7)
 
            // Wait until the server socket is closed.
            // In this example, this does not happen, but you can do that to gracefully
            // shut down your server.
            f.channel().closeFuture().sync();
        } finally {
            workerGroup.shutdownGracefully();
            bossGroup.shutdownGracefully();
        }
    }
 
    public static void main(String[] args) throws Exception {
        int port;
        if (args.length > 0) {
            port = Integer.parseInt(args[0]);
        } else {
            port = 8080;
        }
        new DiscardServer(port).run();
    }
}
```

> 1. `NioEventLoopGroup`是处理`I/O`操作的多线程事件循环，第一个通常称为“`boss`”，接受传入连接。 第二个通常称为“`worker`”，当“`boss`”接受连接并且向“`worker`”注册接受连接，则“`worker`”处理所接受连接的流量。 使用多少个线程以及如何将它们映射到创建的通道取决于`EventLoopGroup`实现，甚至可以通过构造函数进行配置
> 2. `ServerBootstrap`是一个用于设置服务器的助手类。 您可以直接使用通道设置服务器。 但是，请注意，这是一个冗长的过程，在大多数情况下不需要这样做。
> 3. 此处指定的处理程序将始终由新接受的通道计算。 `ChannelInitializer`是一个特殊的处理程序，用于帮助用户配置新的通道。 很可能要通过添加一些处理程序(例如`DiscardServerHandler`)来配置新通道的`ChannelPipeline`来实现您的网络应用程序。 随着应用程序变得复杂，可能会向管道中添加更多处理程序，并最终将此匿名类提取到顶级类中
> 4. 你注意到`option()`和`childOption()`没有？ `option()`用于接受传入连接的`NioServerSocketChannel`。`childOption()`用于由父`ServerChannel`接受的通道，在这个示例中为`NioServerSocketChannel`

###### ChannelOption

> https://blog.csdn.net/qq_31391225/article/details/109513184?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165707207416781647553838%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165707207416781647553838&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-109513184-null-null.142^v31^pc_rank_34,185^v2^tag_show&utm_term=channeloption&spm=1018.2226.3001.4187

```java
// 可以在try中加入信息的处理逻辑
try {
        while (in.isReadable()) { // (1)
            System.out.print((char) in.readByte());
            System.out.flush();
        }
    } finally {
        ReferenceCountUtil.release(msg); // (2)
}
```

###### 测试

```shell
telnet localhost 8080
```

## 时间服务器

###### Handler提供时间

```java
package cn.sxt.netty.time;
 
public class TimeServerHandler extends ChannelInboundHandlerAdapter {
 
    @Override
    public void channelActive(final ChannelHandlerContext ctx) { // (1)
        final ByteBuf time = ctx.alloc().buffer(4); // (2)
        time.writeInt((int) (System.currentTimeMillis() / 1000L + 2208988800L));
 
        final ChannelFuture f = ctx.writeAndFlush(time); // (3)
        f.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture future) {
                assert f == future;
                ctx.close();
            }
        }); // (4)
    }
 
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }}
```

> 1. 我们要写入一个32位整数，因此需要一个[ByteBuf](http://netty.io/4.0/api/io/netty/buffer/ByteBuf.html)，其容量至少为`4`个字节。 通过`ChannelHandlerContext.alloc()`获取当前的`ByteBufAllocator`并分配一个新的缓冲区

###### 编写服务端来发送时间

```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelOption;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;

public class TimeServer {
    private int port;

    public TimeServer(int port){
        this.port = port;
    }

    public void run() throws Exception{
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try{
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup,workerGroup)
                    .channel(NioServerSocketChannel.class)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        public void initChannel(SocketChannel ch) throws  Exception{
                            ch.pipeline().addLast(new TimeServerHandler());
                        }
                    })
                    .option(ChannelOption.SO_BACKLOG,128)
                    .childOption(ChannelOption.SO_KEEPALIVE, true);
            ChannelFuture f = b.bind(port).sync();
            f.channel().closeFuture().sync();
        }finally {
            workerGroup.shutdownGracefully();
            bossGroup.shutdownGracefully();
        }
    }

    public static void main(String[] args) throws Exception {
        int port;
        if(args.length>0){
            port = Integer.parseInt(args[0]);
        }else{
            port = 8080;
        }
        new TimeServer(port).run();
    }
}

```

###### 编写一个handlerd的客户端来获取服务端给的时间

```java
package cn.sxt.netty.time;
 
public class TimeClient {
    public static void main(String[] args) throws Exception {
        String host = args[0];
        int port = Integer.parseInt(args[1]);
        EventLoopGroup workerGroup = new NioEventLoopGroup();
 
        try {
            Bootstrap b = new Bootstrap(); // (1)
            b.group(workerGroup); // (2)
            b.channel(NioSocketChannel.class); // (3)
            b.option(ChannelOption.SO_KEEPALIVE, true); // (4)
            b.handler(new ChannelInitializer<SocketChannel>() {
                @Override
                public void initChannel(SocketChannel ch) throws Exception {
                    ch.pipeline().addLast(new TimeClientHandler());
                }
            });
 
            // Start the client.
            ChannelFuture f = b.connect(host, port).sync(); // (5)
 
            // Wait until the connection is closed.
            f.channel().closeFuture().sync();
        } finally {
            workerGroup.shutdownGracefully();
        }
    }
}
```

###### 编写客户端handler来处理获取到的信息

```java
package cn.sxt.netty.time;
 
import java.util.Date;
 
public class TimeClientHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
                ByteBuf m = (ByteBuf) msg; // (1)
        try {
            long currentTimeMillis = (m.readUnsignedInt() - 2208988800L) * 1000L;
            Date currentTime = new Date(currentTimeMillis);
            System.out.println("Default Date Format:" + currentTime.toString());
 
            SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            String dateString = formatter.format(currentTime);
            // 转换一下成中国人的时间格式
            System.out.println("Date Format:" + dateString);
 
            ctx.close();
        } finally {
            m.release();
        }
    }
 
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}
```

## Decoder

![image-20220706102004785](/img/netty/image-20220706102004785.png)

###### 1. 修改接受信息的Handler-加缓存区

```java
if (buf.readableBytes() >= 4) { // (3)
            long currentTimeMillis = (buf.readUnsignedInt() - 2208988800L) * 1000L;
            System.out.println(new Date(currentTimeMillis));
            ctx.close();
}
```

###### 2. 加入一个DecoderHandler在发送数据和接收数据handler之间再加一层

```java
package cn.sxt.netty.time;
 
public class TimeDecoder extends ByteToMessageDecoder { // (1)
    @Override
    protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) { // (2)
        if (in.readableBytes() < 4) {
            return; // (3)
        }
 
        out.add(in.readBytes(4)); // (4)
    }
}
```

> 1. `ByteToMessageDecoder`是`ChannelInboundHandler`的一个实现，它使得处理碎片问题变得容易。
> 2. 如果`decode()`将对象添加到`out`，则意味着解码器成功地解码了消息。 `ByteToMessageDecoder`将丢弃累积缓冲区的读取部分。要记住，不需要解码多个消息。 `ByteToMessageDecoder`将继续调用`decode()`方法，直到它没有再有任何东西添加

## 使用pojo，而非bytebuf更加优雅

```java
@Override
protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) {
    if (in.readableBytes() < 4) {
        return;
    }
 
    out.add(new UnixTime(in.readUnsignedInt()));
}
```

###### 也可以用在两个handler层之间传递数据的时候--writeAndflush()

###### 同样的我们可以加入一层encode来解码

```java
public class TimeEncoder extends MessageToByteEncoder<UnixTime> {
    @Override
    protected void encode(ChannelHandlerContext ctx, UnixTime msg, ByteBuf out) {
        out.writeInt((int)msg.value());
    }
}
```

