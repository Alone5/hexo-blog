---
title: java-dos
date: 2020-01-13 08:44:14
tags: github
---

### java实现操作dos命令的两种方式

#### 1. 读取文件中的命令
```bash
public class cmd {
       public static void main(String[] args) {
       String path = "D:\\cmd.bat";
       Runtime run = Runtime.getRuntime();
       try {
            //run.exec("cmd /k shutdown -s -t 3600");
            Process process = run.exec("cmd.exe /k start " + path);
                InputStream in = process.getInputStream();
                while (in.read() != -1) {
                    System.out.println(in.read());
                }
                in.close();
                process.waitFor();
            } catch (Exception e) {
                e.printStackTrace();
            }
    }
```

#### 2. 直接读取命令
```java
public class qumf {    
    public static void main(String[] args) {  
      try {  
              Runtime rt = Runtime.getRuntime();  
              Process pr = rt.exec("cmd /c ping www.baidu.com && dir");
              //Process pr = rt.exec("D:\\xunlei\\project.aspx");    
              BufferedReader input = new BufferedReader(new InputStreamReader(pr.getInputStream(), "GBK"));  
              String line = null;  
              while ((line = input.readLine()) != null) {  
                  System.out.println(line);  
              }  
              int exitVal = pr.waitFor();  
              System.out.println("Exited with error code " + exitVal);  
           } catch (Exception e) {  
             System.out.println(e.toString());  
             e.printStackTrace();  
        }  
    }  
```

### 补充，ffmpeg命令存入List 调用线程处理命令

#### 一个简单案例 ffmpeg实现截视频首帧图片
```java
/**
     * 视频截图功能
     *
     * @param sourceVideoPath 需要被截图的视频路径（包含文件名和后缀名）
     * @return
     */
    public boolean processImg(String sourceVideoPath) {

        //先确保保存截图的文件夹存在
        File TempFile = new File(imageRealPath);
        if (TempFile.exists()) {
            if (TempFile.isDirectory()) {
                System.out.println("该文件夹存在。");
            } else {
                System.out.println("同名的文件存在，不能创建文件夹。");
            }
        } else {
            System.out.println("文件夹不存在，创建该文件夹。");
            TempFile.mkdir();
        }

        File fi = new File(sourceVideoPath);
        filename = fi.getName();            //获取视频文件的名称。
        //获取视频名+不加后缀名 后面加.toLowerCase()转为小写
        filerealname = filename.substring(0, filename.lastIndexOf(".")); 

        List<String> commend = new ArrayList<>();
        //第一帧： 00:00:01
        //截图命令：time ffmpeg -ss 00:00:01 -i test1.flv -f image2 -y test1.jpg
        commend.add(ffmpegpath);            //指定ffmpeg工具的路径
        commend.add("-ss");
        commend.add("00:00:01");            //1是代表第1秒的时候截图
        commend.add("-i");
        commend.add(sourceVideoPath);        //截图的视频路径
        commend.add("-f");
        commend.add("image2");
        commend.add("-y");
        commend.add(imageRealPath + filerealname + ".jpg"); //生成截图xxx.jpg

        //打印截图命令
        StringBuffer test = new StringBuffer();
        for (int i = 0; i < commend.size(); i++) {
            test.append(commend.get(i) + " ");
        }
        System.out.println("截图命令:" + test);

        //转码后完成截图功能-还是得用线程来解决
        try {
            //调用线程处理命令
            ProcessBuilder builder = new ProcessBuilder();
            builder.command(commend);
            Process p = builder.start();

            //获取进程的标准输入流
            final InputStream is1 = p.getInputStream();
            //获取进程的错误流
            final InputStream is2 = p.getErrorStream();
            //启动两个线程，一个线程负责读标准输出流，另一个负责读标准错误流
            new Thread() {
                public void run() {
                    BufferedReader br = new BufferedReader(
                            new InputStreamReader(is1));
                    try {
                        String lineB = null;
                        while ((lineB = br.readLine()) != null) {
                            if (lineB != null) {
                            	//必须取走线程信息避免堵塞
                                //System.out.println(lineB);    
                            }
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                    //关闭流
                    finally {
                        try {
                            is1.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }

                }
            }.start();
            new Thread(new Runnable() {
                public void run() {
                    BufferedReader br2 = new BufferedReader(
                            new InputStreamReader(is2));
                    try {
                        String lineC = null;
                        while ((lineC = br2.readLine()) != null) {
                            if (lineC != null) {
                            	//必须取走线程信息避免堵塞
                                //System.out.println(lineC);   
                            }
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                    //关闭流
                    finally {
                        try {
                            is2.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }

                }
            }).start();
            // 等Mencoder进程转换结束，再调用ffmepg进程非常重要！！！
            p.waitFor();
            System.out.println("截图进程结束");
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }
```


