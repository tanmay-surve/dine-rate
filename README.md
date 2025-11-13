**Demo Video:**  
[![YouTube](https://img.icons8.com/?size=50&id=19318&format=png)](https://www.youtube.com/watch?v=VRIpx7-ufWQ)


<img width="1537" height="941" alt="Screenshot From 2025-07-29 22-32-58" src="https://github.com/user-attachments/assets/785de819-cf41-4e99-8037-5010ce0174c2" />
<img width="1537" height="941" alt="Screenshot From 2025-07-29 22-34-42" src="https://github.com/user-attachments/assets/264eab0d-cb2c-4b78-8413-86dd8f4b62e1" />



# 🍽️ Restaurant Review Platform

Welcome to the **Restaurant Review Platform**, a modern web application designed to help users discover local restaurants, read authentic reviews, and share their dining experiences. Built with a robust tech stack, this project combines **Spring Boot**, **Next.js**, **Elasticsearch**, and **Keycloak** to deliver a seamless and secure user experience.

---

## 📖 Overview

The Restaurant Review Platform empowers users to explore restaurants, post reviews with ratings and photos, and leverage advanced search capabilities to find the perfect dining spot. This intermediate-level project showcases full-stack development with a focus on secure authentication, geospatial search, and efficient data management.


---

## ✨ Features

- **Restaurant Discovery**: Search restaurants by query, cuisine type, location, or rating.
- **Review Management**: Create, read, update (within 48 hours), and delete reviews with text, ratings, and photos.
- **Geolocation**: Utilize geospatial search to find restaurants near a specific location.
- **Photo Upload**: Upload and retrieve photos for restaurants and reviews securely.
- **Authentication**: Secure user authentication and authorization using Keycloak with OAuth2 and OpenID Connect.
- **Advanced Search**: Powered by Elasticsearch for fast and flexible text and geospatial queries.

---

## 🛠️ Prerequisites

To set up and run the project, ensure you have the following tools installed:

- **Java**: JDK 21 or later (`java -version` to verify)
- **Node.js**: Version 20 or later (`node -v` to verify)
- **Docker**: Required for Elasticsearch and Keycloak (`docker --version` to verify)
- **Maven**: Bundled with the Spring Boot project, no separate installation needed
- **IDE**: IntelliJ IDEA (Community Edition) recommended, but Visual Studio Code or others work too

---

## 🚀 Project Setup

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/restaurant-review-platform.git
cd restaurant-review-platform
```

### 2. Backend Setup
1. **Create Spring Boot Project**:
   - Visit [Spring Initializr](https://start.spring.io/) and generate a project with:
     - **Language**: Java 21
     - **Build Tool**: Maven
     - **Packaging**: Jar
     - **Dependencies**: Spring Web, Spring Security, Spring Data Elasticsearch, Validation, Lombok
   - Download the generated ZIP file and extract it to your workspace.
   - Open the project in your preferred IDE.

2. **Configure Elasticsearch and Keycloak**:
   - Create a `docker-compose.yml` file in the project root to set up Elasticsearch and Keycloak. Ensure it includes configurations for:
     - Elasticsearch (port 9200) and Kibana (port 5601)
     - Keycloak (port 9090)
   - Start the services in the background:
     ```bash
     docker-compose up -d
     ```
   - Access Kibana at `http://localhost:5601` to manage Elasticsearch indexes.
   - Access the Keycloak admin console at `http://localhost:9090` (default credentials: `admin/admin`).

3. **Configure Spring Boot**:
   - Add the following to `src/main/resources/application.properties`:
     ```properties
     spring.security.oauth2.resourceserver.jwt.issuer-uri=http://localhost:9090/realms/restaurant-review
     ```
   - Update `pom.xml` to include MapStruct and Lombok dependencies for annotation processing:
     ```xml
     <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>${lombok.version}</version>
         <scope>provided</scope>
     </dependency>
     <dependency>
         <groupId>org.mapstruct</groupId>
         <artifactId>mapstruct</artifactId>
         <version>${mapstruct.version}</version>
     </dependency>
     ```
   - Configure the Maven Compiler Plugin for MapStruct and Lombok compatibility:
     ```xml
     <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-compiler-plugin</artifactId>
         <configuration>
             <annotationProcessorPaths>
                 <path>
                     <groupId>org.projectlombok</groupId>
                     <artifactId>lombok</artifactId>
                     <version>${lombok.version}</version>
                 </path>
                 <path>
                     <groupId>org.mapstruct</groupId>
                     <artifactId>mapstruct-processor</artifactId>
                     <version>${mapstruct.version}</version>
                 </path>
             </annotationProcessorPaths>
         </configuration>
     </plugin>
     ```

4. **Run the Backend**:
   - Build and run the Spring Boot application:
     ```bash
     mvn spring-boot:run
     ```

### 3. Frontend Setup
1. **Download Frontend Source**:
   - Obtain the frontend source code from the provided ZIP file or the project's Discord channel.
   - Extract the ZIP to a `frontend` directory in your project root.

2. **Install Dependencies**:
   - Navigate to the frontend directory:
     ```bash
     cd frontend
     npm install
     ```

3. **Run the Frontend**:
   - Start the Next.js development server:
     ```bash
     npm run dev
     ```
   - Access the application at `http://localhost:3000`.

---

## 📂 Project Structure

### Backend
- **Main Components**: Follows standard Spring Boot layout (`src/main/java`).
- **Dependencies**: Defined in `pom.xml`, including Spring Boot, Elasticsearch, Validation, and Lombok.
- **Entities**: User, Address, OperatingHours, Photo, Review, Restaurant.
- **Repositories**: `RestaurantRepository` for CRUD and search operations.
- **Services**: Handle business logic for restaurant/review creation, updates, deletions, and searches.
- **Controllers**: REST API endpoints for restaurants, reviews, and photos.
- **Security**: Configured with Spring Security and Keycloak for JWT-based authentication.
- **File Storage**: Manages photo uploads/retrieval with custom exceptions.

### Frontend
- Built with **Next.js**, located in the `frontend` directory.
- Communicates with the backend via REST API endpoints.

---

## 🌐 REST API Endpoints

### Restaurants
- **GET /api/restaurants**: Search restaurants with query parameters (`q`, `latitude`, `longitude`, `radius`, `cuisineType`, `minRating`, `page`, `size`).
- **POST /api/restaurants**: Create a new restaurant (auth required).
- **GET /api/restaurants/{restaurantId}**: Retrieve restaurant details.
- **PUT /api/restaurants/{restaurantId}**: Update restaurant details (auth required).
- **DELETE /api/restaurants/{restaurantId}**: Delete a restaurant (auth required).

### Reviews
- **POST /api/restaurants/{restaurantId}/reviews**: Create a review (auth required).
- **GET /api/restaurants/{restaurantId}/reviews**: List reviews with pagination and sorting.
- **GET /api/restaurants/{restaurantId}/reviews/{reviewId}**: Retrieve a single review.
- **PUT /api/restaurants/{restaurantId}/reviews/{reviewId}**: Update a review (auth required, within 48 hours).
- **DELETE /api/restaurants/{restaurantId}/reviews/{reviewId}**: Delete a review (auth required).

### Photos
- **POST /api/photos**: Upload a photo (auth required, `multipart/form-data`).
- **GET /api/photos/{id}**: Retrieve a photo (public access).

---

## 🧪 Testing

- **Manual Testing**: Use `RestaurantDataLoaderTest` to import sample restaurant data for development and testing.
- **UI Testing**: Verify functionality through the frontend at `http://localhost:3000`.
- **API Testing**: Test endpoints using tools like Postman or curl.

---
