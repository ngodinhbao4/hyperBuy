# GIAI ĐOẠN 1: XÂY DỰNG ỨNG DỤNG VỚI MAVEN
# Sử dụng một base image Maven với OpenJDK 21 từ eclipse-temurin
FROM maven:3.9.6-eclipse-temurin-21-alpine AS build

# Đặt thư mục làm việc bên trong container
WORKDIR /app

# Sao chép file pom.xml vào thư mục làm việc
COPY pom.xml .

# Tải tất cả các dependency được định nghĩa trong pom.xml
RUN mvn dependency:go-offline -B

# Sao chép toàn bộ mã nguồn của ứng dụng vào thư mục làm việc
COPY src ./src

# Xây dựng ứng dụng, tạo file .jar
RUN mvn package -DskipTests -B


# GIAI ĐOẠN 2: CHẠY ỨNG DỤNG
# Sử dụng một base image OpenJDK JRE 21 từ eclipse-temurin nhỏ gọn hơn
FROM eclipse-temurin:21-jre-alpine

# Đặt thư mục làm việc bên trong container
WORKDIR /app

# Sao chép file .jar đã được build từ giai đoạn 'build' vào thư mục làm việc của giai đoạn hiện tại
# Tên file JAR dựa trên artifactId và version trong pom.xml (ví dụ: user-service-0.0.1-SNAPSHOT.jar)
COPY --from=build /app/target/*.jar app.jar

# Mở cổng mà ứng dụng Spring Boot của bạn đang lắng nghe (8080 theo application.properties)
EXPOSE 8080

# Lệnh để chạy ứng dụng khi container được khởi động
ENTRYPOINT ["java", "-jar", "app.jar"]
