---
title: 练习案例-6
date: 2019-05-14 20:55:18
tags: github
---
## Eclipse控制台简易聊天程序（仅限同一局域网）
### 服务端程序
```java
package socket;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Arrays; 

/**
 * 聊天室的服务端
 * @author soft01
 *
 */
public class Server {
	/*
	 * java.net.ServerSocket
	 * 运行在服务端的ServerSocket主要有两个作用
	 * 1：向系统申请服务端口，客户端就是通过这个端口与服务器建立连接的。
	 * 2：监听该端口，这样一旦一个客户端通过该端口尝试建立连接时，
	 * ServerSocket就会自动实例化一个Socket，
	 * 那么通过Socket就可以与客户端对等并进行数据交互了。
	 */
	private ServerSocket server;
	/*
	 * 该数组用于保存所有ClientHandler对应客户端的输出流，
	 * 以便所有ClientHandler都可以互访这些输出流来广播消息。
	 */
	private PrintWriter[] allOut = {};
	
	public Server() {
		try {
			/*
			 * 实例化ServerSocket的同时要向系统申请服务端口,
			 * 客户端就是通过这个端口与服务端建立连接的。
			 */
			System.out.println("正在启动服务端...");
			server = new ServerSocket(8088);
			System.out.println("服务端启动完毕！");
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public void start() {
		try {
			/*
			 * Socket accept()
			 * ServerSocketde的accept方法返回是一个阻塞方法，
			 * 调用后会一直等待，知道一个客户端建立连接为止，
			 * 此时该方法会返回一个Socket实例，通过这个Socket就
			 * 可以与刚连接上的客户端进行数据交互了。
			 * 多次调用accept可以等待不同客户端的连接
			 */
			System.out.println("等待客户端连接...");
			Socket socket = server.accept();
			System.out.println("一个客户端连接了!");
			
			//启动一个线程负责与客户端交互
			ClientHandler handler = new ClientHandler(socket);
			Thread t = new Thread(handler);
			t.start();
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		Server server = new Server();
			server.start();
	}
	/**
	 * 该线程任务是负责与指定客户端进行交互操作
	 */
	private class ClientHandler implements Runnable{
		private Socket socket;
		private String host;
		
		public ClientHandler(Socket socket) {
			this.socket = socket;
			//获取远端计算机IP地址信息的字符串格式
			host = socket.getInetAddress().getHostAddress();
		}
		
		public void run() {
			PrintWriter pw = null;
			try {
				/*
				 * 通过Socket获取输入流，可以读取到远端计算机发送过来的字节。
				 */
				InputStream in = socket.getInputStream();
				InputStreamReader isr = new InputStreamReader(in,"UTF-8");
				BufferedReader br = new BufferedReader(isr);
				/*
				 * 通过socket获取输出流，用于给客户段发送消息。
				 */
				OutputStream out = socket.getOutputStream();
				OutputStreamWriter osw = new OutputStreamWriter(out,"UTF-8");
				BufferedWriter bw = new BufferedWriter(osw);
				pw = new PrintWriter(bw,true);
				/*
				 * 将当前客户端对应的输出流存入allOut，
				 * 以便于其他ClientHandler可以将消息转发给这个客户端。
				 */
				synchronized (allOut) {
					// 1.对allOut数组扩容
					allOut = Arrays.copyOf(allOut, allOut.length + 1);
					// 2.将输出流存入数组最后一个位置
					allOut[allOut.length - 1] = pw;
				}
				System.out.println(host+"上线了！");
				System.out.println("当前在线人数为："+allOut.length);
				
				String line = null;
				while((line = br.readLine())!=null) {
					System.out.println(host+"说："+line);
					//将内容发送给客户端
					synchronized (allOut) {
						for (int i = 0; i < allOut.length; i++) {
							allOut[i].println(host + "说:" + line);
						}
					}
				}
			} catch (Exception e) {
			} finally {
				// 处理客户端断开连接的操作
				synchronized (allOut) {
					// 1.将当前客户端输出流从allOut中删除
					for (int i = 0; i < allOut.length; i++) {
						if (allOut[i] == pw) {
							allOut[i] = allOut[allOut.length - 1];
							allOut = Arrays.copyOf(allOut, allOut.length - 1);
						}
					}
				}
				System.out.println(host+"下线了！");
				System.out.println("当前在线人数为："+allOut.length);
				// 2.关闭socket，释放资源
				try {
					socket.close();
				} catch (IOException e) {
				}

			}
		}
	}
}
```
### 客户端程序
```java
package socket;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

/**
 * 聊天室的客户端
 * 
 * @author soft01
 *
 */
public class Client {
	/**
	 * java.net.Socket 套接字 封装了TCP协议的通讯细节， 
	 * 使得我们可以通过它来与远端计算机建立TCP连接， 并利用两个流的读写完成数据交换
	 */
	Scanner scan = new Scanner(System.in);
	private Socket socket;
	/**
	 * 构造方法，用来初始化客户端
	 */
	public Client() {
		try {
			/*
			 * Socket实例化是需要传两个参数 
			 * 参数1：服务端IP地址 
			 * 参数2：服务端申请程序的端口 我们通过IP可以找到网络上的服务端计算机
			 * 通过端口可以连接到服务端计算机上运行的服务端应用程序。 
			 * 注意： 实例化Socket的过程就是连接的过程， 若服务端没有响应这里会抛出异常。
			 */
			System.out.println("正在连接服务端...");
			// localhost 本地服务
			System.out.println("请输入您要连接的IP：");
			socket = new Socket(scan.nextLine(), 8088);
			System.out.println("成功连接服务端！");

		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	/**
	 * 客户端开始工作的方法
	 */
	public void start() {
		try {
			// 用于读取服务端发送过来消息的线程
			ServerHandler handler = new ServerHandler();
			Thread t = new Thread(handler);
			t.start();
			/*
			 * Socket提供的方法： OutputStream getOutputStream() 
			 * 该方法获取的输入流是一个字节输出流
			 * 通过这个流写出的字节会通过网络发送到远端计算机上 (对于客户端这边而言，远端计算机指的就是服务端)
			 */
			OutputStream out = socket.getOutputStream();
			OutputStreamWriter osw = new OutputStreamWriter(out, "UTF-8");
			BufferedWriter bw = new BufferedWriter(osw);
			PrintWriter pw = new PrintWriter(bw, true);

			while (true) {
				String line = scan.nextLine();
				pw.println(line);

			}

		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		Client client = new Client();
		client.start();

	}
	/**
	 * 该线程负责循环读取服务发送过来的消息并输出到客户端的控制台上。
	 * 
	 * @author asus
	 *
	 */
	private class ServerHandler implements Runnable {
		public void run() {
			try {
				// 通过socket获取输入流，读取服务端发送过来的内容。
				InputStream in = socket.getInputStream();
				InputStreamReader isr = new InputStreamReader(in, "UTF-8");
				BufferedReader br = new BufferedReader(isr);

				String line = null;
				while ((line = br.readLine()) != null) {
					System.out.println(line);
				}

			} catch (Exception e) {

			}
		}
	}
}

```