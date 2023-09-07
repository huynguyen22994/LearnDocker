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
Quay lại xem vấn đề này sau

## Use bind mounts
Quay lại xem vấn đề này sau


