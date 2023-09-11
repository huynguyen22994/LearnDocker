# Docker

Docker là một nền tảng mã nguồn mở sử dụng để triển khai và chạy các ứng dụng trong các môi trường cô lập gọi là containers. Containers là một phương pháp ảo hóa mức độ hệ điều hành (OS-level virtualization), cho phép bạn đóng gói ứng dụng cùng với các thư viện và tài nguyên cần thiết vào một container độc lập.

Mỗi container chạy một ứng dụng cụ thể và hoạt động như một môi trường độc lập. Các container này có thể chạy trên bất kỳ hệ điều hành nào hỗ trợ Docker, bao gồm Windows, macOS và các phiên bản Linux.

https://www.docker.com/

## Lợi ích của Docker
- Đóng gói đồng nhất: Docker cho phép bạn đóng gói ứng dụng cùng với các thư viện và tài nguyên cần thiết vào một container, đảm bảo sự đồng nhất và dễ dàng triển khai ứng dụng trên nhiều môi trường khác nhau.
- Tính di động: Container Docker có thể chạy trên bất kỳ hệ điều hành nào hỗ trợ Docker, giúp dễ dàng di chuyển và triển khai ứng dụng trên nhiều môi trường khác nhau.
- Hiệu suất: Containers Docker chạy trực tiếp trên hệ điều hành máy chủ, giúp giảm tài nguyên và tăng hiệu suất so với các phương pháp ảo hóa truyền thống.
- Quản lý tài nguyên: Docker cung cấp các công cụ để quản lý tài nguyên và quản lý mạng cho các container, giúp tối ưu hóa việc triển khai và quản lý ứng dụng.

![](/images/overview.png)

## Cài đặt Docker
- Docker Desktop: https://www.docker.com/get-started/
- Docker Hub: https://www.docker.com/products/docker-hub/

## Docker Hub
Docker Hub được sử dụng để lưu trữ, quản lý và chia sẻ các container Docker.
Docker Hub có thể ví như GitHub. GitHub là nơi lưu trữ respository code trên cloud thì Docker Hub là nơi lưu trữ các container và images trên Cloud.

![](/images/docker-image-contain.png)

## Container
Container là:
    - 1. Là một phiên bản có thể chạy được của một image. Bạn có thể tạo, bắt đầu, dừng, và xóa một container bằng cách dùng Docker API or CLI.
    - 2. Có thể chạy trên local machines, virtual machines, hoặc deployed trên cloud.
    - 3. Có tính di động (và có thể chạy trên mọi hệ điều hành).
    - 4. Các Container chạy độc lập và mỗi container chạy phần mềm, tệp nhị phân, cấu hình, v.v. của riêng nó.
## Image
Image là gì:
    - 1. Một container đang chạy sử dụng một hệ thống tập tin bị cô lập. Hệ thống tệp biệt lập này được cung cấp bởi một Image và Image phải chứa mọi thứ cần thiết để chạy một ứng dụng - tất cả các phần phụ thuộc, cấu hình, tập lệnh, tệp nhị phân, v.v. 
    - 2. Image cũng chứa các cấu hình khác cho vùng chứa, chẳng hạn như biến môi trường, lệnh mặc định để chạy và dữ liệu metadata khác.
## Dockerfile
Dockerfile chỉ đơn giản là một tệp dựa trên văn bản chứa tập lệnh hướng dẫn. Docker sử dụng tập lệnh này để xây dựng một container image.

## Các bước để tạo một Image của ứng dụng
1. Tạo file Dockerfile ngang cấp với package.json
2. Viết hướng dẫn cài đặt phần mềm trong Dockerfile để tạo image. Ví dụ
```
# syntax=docker/dockerfile:1

FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
3. Build một image bằng command:
```
docker build -t getting-started .
```
- Cách chạy của lệnh trên:
    - Khi lệnh trên chạy `docker build` thì docker sẽ bắt đầu tạo ra một image. Và quá trình tạo image sẽ theo hướng dẫn của `Dockerfile` như sau:
    1. Docker sẽ dowload xuống rất nhiều "layers". Bởi vì bắt đầu bằng `node:18-alpine` image nên Docker sẽ down node 18 image về
    2. Sau đó Copy hết tất cả những thứ dowan về vào `/app` được hiểu là ứng dụng của chúng ta trong image.
    3. Tiếp theo là dùng `yarn` để cài đặt các dependencies của app nodejs. Lệnh CMD command `node src/index.js` là lệnh mặc định sẽ chạy khi bắt đầu start một container từ image này.
    4. `-t` là cờ (flag) trong Docker dùng để đặt tên cho image. image tên `getting-started`
    5. `.` trong commmand dùng để nói rằng sẽ chạy file docker tên là `Dockerfile` 

## Cách chạy một Container Ứng Dụng
```
docker run -dp 127.0.0.1:3000:3000 getting-started
```
- Cách chạy của lệnh trên:
    1. Dùng lệnh `docker run` để chạy một container và chỉ định tên `image` (getting-started) mà muốn chạy
    2. Cờ `-d` (viết tắt của `--detach`) chạy container ở chế độ nền
    3. Cờ `-p` (viết tắt của `--publish`) tạo ánh xạ Port giữa máy chủ và container.
    4. Cờ `-p` nhận giá trị chuỗi theo định dạng `HOST:CONTAINER`, trong đó `HOST` là địa chỉ trên máy chủ và `CONTAINER` là Port trên Container.
    5. Lệnh publishes Port 3000 của Container thành `127.0.0.1:3000` (`localhost:3000`) trên host. Nếu không có ánh xạ cổng, bạn sẽ không thể truy cập ứng dụng từ máy chủ Host.

## Xem cách container đang chạy
Để xem các Container đang đang chạy có thể dùng CLI hoặc Docker Desktop để xem:

#### CLI
Để xem bằng CLI dùng lệnh
```
docker ps
```
![](/images/docker-ps.png)

#### Docker Desktop
![](/images/docker-desktop.png)

## Update the source code

Build update một version image mới, sử dụng lệnh `docker build`.
```
docker build -t getting-started .
```

Sau khi build lại thì start lại service
```
docker run -dp 127.0.0.1:3000:3000 getting-started
```

## Dừng Container đang chạy - Xóa container

1. Lấy ID của container bằng command
```
docker ps
```

2. Sử dụng lệnh `docker stop` để stop một container đang chạy. `<the-container-id>` lấy từ docker ps
```
docker stop <the-container-id>
```

3. Khi container dừng thì có thể xóa conatiner đó bằng lệnh `docker rm`
```
docker rm <the-container-id>
```

(*) Có thể thực hiện dừng container và xóa container cùng một lệnh bằng cách thêm `force`
```
docker rm -f <the-container-id>
```

## List image đang có
```
docker image ls
```

## Chia sẽ Image

Sau khi tạo được Image, bạn có thể chia sẻ nó. Để chia sẻ image Docker, bạn phải sử dụng Docker registry. registry mặc định là Docker Hub và là nơi chứa tất cả các image bạn đã sử dụng

#### Create a repository 
To push an image, you first need to create a repository on Docker Hub.

1. Vào Docker Hub.

2. Chọn nút `Create Repository`.

3. For the repository name, use `getting-started`. Make sure the `Visibility` is `Public`.

4. Select `Create`.

Trong hình ảnh bên dưới là ví dụ để đây tag mới vào repository Docker Hub vừa tạo bằng command:
![](/images/push-repo-docker.png)

#### Push the image

1. Chọn tag image để push lên Hub repository getting-started bằng cách Kiểm tra trên local repo có những images nào bằng lệnh
```
docker image ls
```
2. Đăng nhập vào Docker Hub trên command bằng lệnh
```
docker login -u YOUR-USER-NAME
```
3. Sử dụng `docker tage` để đặt lại tên mới cho image getting-started. Thay thế `YOUR-USER-NAME` bằng ID Docker của bạn.
```
docker tag getting-started YOUR-USER-NAME/getting-started
```
4. Bây giờ hãy chạy lại lệnh `docker push`. Nếu bạn đang sao chép giá trị từ Docker Hub, bạn có thể bỏ phần `:tagname` vì bạn chưa thêm tag vào tên image. Nếu bạn không chỉ định tag, Docker sẽ sử dụng thẻ có tên mới nhất.
```
docker push YOUR-USER-NAME/getting-started
```

## Chạy Docker Image trên Docker Play

Docker Play dùng để run image trên môi trường kết nối network. Việc này có thể giúp build 1 sản phẩm demo cho khách hàng test.

![](/images/docker-play-1.png)

## Persist the DB 
Dùng để tách dữ liệu trong container ra lưu riêng để khi update lại một image và run lại không mất dữ liệu. Do mỗi docker container là một không gian độc lập không liên quan đến các container khác nên khi remove và run lại dữ liệu lưu trong file container đó sẽ bị xóa

## Docker volumes
Để giải quyết vấn đề Persist Data, thì dùng volumes để giải quyết. Docker cho phép tạo một ổ đĩa riêng biệt với container bằng lệnh CLI hoặc Docker Desktop. 

![](/images/docker-volumn.png)

Sử dụng Docker Volume
```
1. Tạo một volumn
> docker volume create todo-db

2. Dừng và xóa conatiner ứng dụng việc cần làm một lần nữa bằng `docker rm -f <id>` vì nó vẫn đang chạy mà không sử dụng ổ đĩa liên tục.
> docker rm -f <id>

3. Khởi động container sử dụng volume, thêm tùy chọn --mount để chỉ định volumn gắn kết. Đặt tên cho volumn và gắn nó vào `/etc/todos` trong container, nơi ghi lại tất cả các tệp được tạo tại đường dẫn.
> docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started

(*) trong các trường hợp sau này nếu có sử dụng volume muốn start lại một container sừ dụng volum thì phải thêm option --mount
```

#### Đi sau vào cấu trúc volume:

Rất nhiều người thường xuyên hỏi "Docker lưu trữ dữ liệu của tôi ở đâu khi tôi sử dụng ổ đĩa?" Nếu muốn biết, bạn có thể sử dụng lệnh kiểm tra âm lượng docker.
```
> docker volume inspect todo-db
[
    {
        "CreatedAt": "2023-09-09T06:24:51Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": null,
        "Scope": "local"
    }
]
```
`Mountpoint` là vị trí thực tế của dữ liệu trên đĩa. Lưu ý rằng trên hầu hết các máy, bạn sẽ cần có quyền truy cập root để truy cập thư mục này từ máy chủ.

## Docker bind mounts
Bind Mount là một loại gắn kết giữ thư mục trong container và máy chủ, cho phép bạn chia sẻ thư mục từ hệ thống tệp của máy chủ vào conatiner. Khi làm việc trên một ứng dụng, bạn có thể sử dụng bind mount để gắn mã nguồn vào container. Container sẽ nhìn thấy những thay đổi bạn thực hiện đối với mã ngay lập tức, ngay khi bạn lưu tệp. Điều này có nghĩa là bạn có thể chạy các quy trình trong container để theo dõi các thay đổi của hệ thống tệp và phản hồi chúng.

![](/images/docker-bind-mounts.png)

Ví Dụ dùng bind mount cho getting-started (dùng nodemon để server tự restart)

```
1. Chạy lệnh
> docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
(*) --mount yêu cầu Docker tạo một bind mount, trong đó src là thư mục làm việc hiện tại trên máy chủ của bạn (ứng dụng bắt đầu) và target là nơi thư mục đó sẽ xuất hiện bên trong container (/src).

2. Sau khi chạy lệnh, Docker bắt đầu phiên bash tương tác trong thư mục gốc của hệ thống tệp của vùng chứa.
> root@710fcdd0b1d4:/# ls
> bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  src  srv  sys  tmp  usr  var

3. cd vào folder src
> root@710fcdd0b1d4:/# cd src
> root@710fcdd0b1d4:/src# ls
> Readme.md  getting-started-app  images

4. Tạo file trong container để xem ngoài source code của host có file mới tạo không
> root@710fcdd0b1d4:/src# touch myfile.txt
(ảnh bên dưới)

=> Nếu folder mount trong container thay đổi thì file system của host cũng thay đổi và ngược lại (Nếu dùng bind mount)
```

![](/images/bind-mount-1.png)

### Chạy container trên môi trường development

Các bước để dev trên container 
- Mount your source code into the container
- Install all dependencies
- Start nodemon to watch for filesystem changes

Xem cách thực hiện tại đây:
https://docs.docker.com/get-started/06_bind_mounts/


## Docker network
Bây giờ sẽ thêm MySQL vào ngăn xếp ứng dụng. Câu hỏi sau đây thường được đặt ra - "MySQL sẽ chạy ở đâu? Cài đặt nó trong cùng một vùng chứa hay chạy riêng?" Nói chung mỗi container nên làm một việc và làm tốt. Sau đây là một số lý do để chạy vùng chứa riêng biệt:

![](/images/docker-networks-split.jpg)

### Tạo một container chạy MySQL
Quay lại ví dụ getting-started. Giờ chúng ta sẽ tách container getting-started ra chạy riêng và conatiner MySQL chạy riêng. Và dùng nettwork kết nối chúng lại.
Để tạo một container MySQL với network có thể dùng 2 cách:

    1. Chỉ định mạng khi khởi động container.
    2. Kết nối một container đang chạy tới một mạng

Theo các bước sau để Tạo một mạng rồi chỉ định container MySQl đến mạng đó khi khởi động

1. Tạo network
```
> docker network create todo-app
```

2. Bắt đầu một container MySQL và gán nó vào network. Bạn cũng sẽ xác định một số biến môi trường mà cơ sở dữ liệu sẽ sử dụng để khởi tạo cơ sở dữ liệu.
```
> docker run -d `
    --network todo-app --network-alias mysql `
    -v todo-mysql-data:/var/lib/mysql `
    -e MYSQL_ROOT_PASSWORD=secret `
    -e MYSQL_DATABASE=todos `
    mysql:8.0
```

3. Để xác nhận rằng bạn đã thiết lập và chạy cơ sở dữ liệu, hãy kết nối với cơ sở dữ liệu và xác minh rằng nó kết nối.
```
> docker exec -it <mysql-container-id> mysql -u root -p

and then run
> mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| todos              |
+--------------------+
5 rows in set (0.01 sec)
```
4. Thoát khỏi shell MySQL để quay lại shell trên máy của bạn.
```
mysql> exit
```
### Kết nối tới MySQL
Các container sẽ chạy các Ip khác nhau.

1. Sử dụng container mới dùng image nicolaka/netshoot. Để đảm bảo rằng các container có chung một mạng
```
> docker run -it --network todo-app nicolaka/netshoot
```
2. Bên trong vùng chứa, bạn sẽ sử dụng lệnh dig, đây là một công cụ DNS hữu ích. Bạn sẽ tra cứu địa chỉ IP cho tên máy chủ mysql.
```
> dig mysql
; <<>> DiG 9.18.13 <<>> mysql
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26083
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;mysql.                         IN      A

;; ANSWER SECTION:
mysql.                  600     IN      A       172.18.0.2

;; Query time: 0 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Mon Sep 11 02:58:25 UTC 2023
;; MSG SIZE  rcvd: 44
```
Mặc dù mysql thường không phải là tên máy chủ hợp lệ nhưng Docker có thể phân giải nó thành địa chỉ IP của container có network alias. Hãy nhớ rằng trước đó bạn đã sử dụng --network-alias.
Điều này có nghĩa là ứng dụng của bạn chỉ cần kết nối với máy chủ có tên mysql và nó sẽ giao tiếp với cơ sở dữ liệu.

### Chạy app getting-started với MySQL
Cài đặt một số biến môi trường để kết nối MySQL.
- MYSQL_HOST - the hostname for the running MySQL server
- MYSQL_USER - the username to use for the connection
- MYSQL_PASSWORD - the password to use for the connection
- MYSQL_DB - the database to use once connected

1. Đứng tại source code getting-started và run command
```
> docker run -dp 127.0.0.1:3000:3000 `
  -w /app -v "$(pwd):/app" `
  --network todo-app `
  -e MYSQL_HOST=mysql `
  -e MYSQL_USER=root `
  -e MYSQL_PASSWORD=secret `
  -e MYSQL_DB=todos `
  node:18-alpine `
  sh -c "yarn install && yarn run dev"
```

2. Để vào xem log container mới start
```
> docker logs -f <container-id>
```
3. Mở chrome vào port 3000 trên local và test app
4. Vào container mysql xem dữ liệu
```
> docker exec -it <mysql-container-id> mysql -p todos

> mysql> select * from todo_items;
+--------------------------------------+----------+-----------+
| id                                   | name     | completed |
+--------------------------------------+----------+-----------+
| 68e5335b-039b-4af3-8a7c-a87c9693fd1d | huy test |         0 |
+--------------------------------------+----------+-----------+
```
Bảng của bạn sẽ trông khác vì nó có các mục của bạn. Tuy nhiên, bạn sẽ thấy chúng được lưu trữ ở đó.

## Docker Compose
Docker Compose là một công cụ giúp bạn xác định và chia sẻ các ứng dụng nhiều containers. Với Compose, bạn có thể tạo tệp YAML để xác định các dịch vụ và chỉ bằng một lệnh duy nhất, bạn có thể xoay mọi thứ lên hoặc chia nhỏ tất cả.

Ưu điểm lớn của việc sử dụng Compose là bạn có thể xác định ngăn xếp ứng dụng của mình trong một tệp, giữ nó ở thư mục gốc của kho dự án (hiện đã được kiểm soát phiên bản) và dễ dàng cho phép người khác đóng góp cho dự án của bạn. Ai đó chỉ cần sao chép kho lưu trữ của bạn và khởi động ứng dụng bằng Compose.
![](/images/docker-compose.png)

### Bắt đầu tạo một Compose
Để thực hiện Docker Compose việc đầu tiên là tạo file .yaml ở root source để ghi cách lệnh cho Docker `compose.yaml`

File `compose.yaml`
```
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 127.0.0.1:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

Để run file compose chạy lệnh
```
> docker compose up -d
```

## Những lệnh Docker thường dùng

```
# Kiểm tra version docker
> docker -v

# Kiểm tra container đang chạy
> docker ps

# Kiểm tra tất cả container đang có kể cả không chạy
> docker ps -a

# Chạy một image
> docker run -dp 127.0.0.1:3000:3000 <Tên Image>
hoặc gán lại tên khi chạy để dể thao tác với tên thay vì ID
> docker run --name <Tên Muốn Đặt Khi Run> -dp 127.0.0.1:3000:3000 <Tên Images Đang Có>
Có thể kéo một image trên hub về chạy với lệnh run Ví dụ run redis như sau
> docker run --name rdb -dp 6379:6379 redis
Chạy với volume
> docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started

# Stop một container đang chạy bằng name hoặc ID
> docker stop <Tên Khi Chạy hoặc Image ID>

# Build image cho container
> docker build -t <Image Name> .

# Xem danh sách những Images đang có
> docker images

# Xóa một image
> docker rm <the-container-id>

# Xóa một image ngay khi đang chạy với force
> docker rm -f <the-container-id>

# Access vào cmd của một contain để thao tác lệnh trên Docker
> docker exec -it < Tên Container hoặc ID >
> docker exec -it < Tên Container hoặc ID > sh
ví dụ:
> docker exec -it rdb sh

# Để kiểm tra image trên hub của các tool cài cho docker có thể dùng lệnh
> docker search < từ khóa image muốn tìm kiếm >

# Tạo một volumn
> docker volume create todo-db

# Inspect một volume
> docker volume inspect todo-db

# Xem log một container đang chạy
> docker logs -f <id || name>

# Sử dụng bind mount cho container
> docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash

# Tạo network
> docker network create todo-app

# List danh sách network
> docker network ls

# Check Ip của một container
> docker inspect <container ID>
> docker inspect <container id> | findstr "IPAddress"

# Run Compose
> docker compose up -d

```
## Docker Servies
## Docker Swarm

