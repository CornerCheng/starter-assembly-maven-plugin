# starter-assembly-maven-plugin

assembly maven plugin for java projects / Java项目的打包插件

## demo

https://github.com/knightliao/starter-assembly-demo

## 使用方式

    <build>

        <finalName>starter-assembly-demo</finalName>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>

        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.example.starter.DemoMain</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>

            <plugin>
                <groupId>com.knightliao.plugin</groupId>
                <artifactId>starter-assembly-maven-plugin</artifactId>
                <version>1.0.0-SNAPSHOT</version>
                <configuration>
                    <appendAssemblyId>false</appendAssemblyId>
                    <finalName>${project.build.finalName}</finalName>
                </configuration>
            </plugin>
        </plugins>

    </build>

## 效果

### 打包

mvn clean package starter:bin

### 结果

#### first

    ➜  target git:(master) ls -l
    total 25408
    drwxr-xr-x  2 knightliao  staff        68  6  3 00:20 archive-tmp
    drwxr-xr-x  7 knightliao  staff       238  6  3 00:20 classes
    drwxr-xr-x  3 knightliao  staff       102  6  3 00:20 maven-archiver
    -rw-r--r--  1 knightliao  staff      5009  6  3 00:20 starter-assembly-demo.jar
    -rw-r--r--  1 knightliao  staff  13000346  6  3 00:20 starter-assembly-demo.tar.gz
    drwxr-xr-x  6 knightliao  staff       204  6  3 00:20 starter-run

其中

- starter-run 是包含所有可执行脚本以及jar包的文件目录 
- starter-assembly-demo.tar.gz 是对 starter-run 目录的打包

#### second

    ➜  target git:(master) cd starter-run
    ➜  starter-run git:(master) ls -l
    total 16
    drwxr-xr-x   5 knightliao  staff   170  6  3 00:20 conf
    drwxr-xr-x  14 knightliao  staff   476  6  3 00:20 lib
    -rw-r--r--   1 knightliao  staff  1160  6  3 00:20 start.sh
    -rw-r--r--   1 knightliao  staff   542  6  3 00:20 stop.sh

其中

- start.sh 是开始脚本, 插件自动生成的
- stop.sh 是关闭脚本, 插件自动生成的
- lib 包含所有jar包
- conf 包含所有配置

    ➜  starter-run git:(master) ls conf
    demo.properties env             logback.xml
    
其中

- env 是启动前的环境变量设置

    ➜  conf git:(master) cat env
    
    BUNDLE_JAR_NAME=starter-assembly-demo.jar
    
    export JAVA_OPTS="$JAVA_OPTS -server -Xms1024m -Xmx1024m -Xmn448m -Xss256K -XX:MaxPermSize=128m -XX:ReservedCodeCacheSize=64m"
    export JAVA_OPTS="$JAVA_OPTS -XX:+UseParallelGC -XX:+UseParallelOldGC -XX:ParallelGCThreads=2"
    export JAVA_OPTS="$JAVA_OPTS -XX:+PrintGCDetails -XX:+PrintGCTimeStamps"
    export JAVA_OPTS="$JAVA_OPTS -Dlogback.configurationFile=file:conf/logback.xml"

其中

- 必须设置 BUNDLE_JAR_NAME ，否则 start.sh 无法启动
- 其它参数可配

#### third

    ➜  starter-run git:(master) sh start.sh
    nohup java  -server -Xms1024m -Xmx1024m -Xmn448m -Xss256K -XX:MaxPermSize=128m -XX:ReservedCodeCacheSize=64m -XX:+UseParallelGC -XX:+UseParallelOldGC -XX:ParallelGCThreads=2 -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Dlogback.configurationFile=file:conf/logback.xml -Djava.ext.dirs=/Users/knightliao/baiduyun/dev/git_java/starter-assembly-demo/target/starter-run/conf:/Users/knightliao/baiduyun/dev/git_java/starter-assembly-demo/target/starter-run/lib -jar lib/starter-assembly-demo.jar  >> log_1464884529.log 2>&1 &
    ➜  starter-run git:(master) sh stop.sh
    Find process and pid=[39632]
    Kill pid=[39632] done
    




