# CI/CD

## Konsep Dasar
**Jenkins**

Beberapa cluster, ingin deploy sekaligus, combine ifra dengan terraform. 

Pipeline as Code : pakai Groovy

Dockerfile prosedur untuk setup docker image dan bukan bahas pemrograman
Jenkinsfile pakai bahasa groovy. supaya memudahkan kita, fleksibel, dan bisa di custom dan code nya read able. Bisa disimpan di souce code atau di jenkins nya.

Declarative : build maven, 
Scripted : kompleks, jika kita jalankan di windows (bat) bisa dikaish command, menjalankan command advanced.

Shared library : konfigurasi code yang bisa di shared, supaya tidak duplicate antar jenkinsfile yang lain. Untuk menjaga supaya style code kita tetap sama. 

Parallel Build : untuk menghemat build time secara efisien untuk menjalani seluruh step.
contoh : antara build dan stage ga cocok. yang cocok saat menjalankan tes dan static analyst

Matrix Build :

**Tips Keamanan**

- Jangan hardcode secret dalam Jenkinsfile
- Gunakan withCredentials{} block
- Gunakan Vault Plugin atau integrasi dengan HashiCorp Vault untuk skenario skala besar
- Putar credential secara berkala
- Audit penggunaan credential (melalui log atau plugin audit)

## Jenkins Trigger
Gimana cara kita untuk mentrigger jenkins untuk melakukan job di pipeline.

## Git
Version control system : kita bisa manage, bisa atur kapan harus liris. semua collaborator bisa tercatat siapa yang merubah, dll.

Kita butuh git untuk kolaborasi, supaya code kita bisa di share ke orang lain.

git hosting : git untuk menaruh source code kita disana. contoh github atau gitlab. gitee on premise tidak ada limit

SCM Pooling : setiap 5 menit sekali langsung trigger untuk mengecek adakah perubahan atau tidak, jika tidak ada akan di ignore.

## Unit Testing
Di springboot punya controller. Di controller ada namanya get Mapping

Gherkin
Given
When
Then

Scenario : Succesfully accessing /hello endpoint
Given the Spring Boot application is running
When I send a GET request to "/hello"

Integration Tesing : Springboot conect ke database. Integrasi adalah dua entitas yang saling terhubung. melewati data access layer. untuk menguji apakah benar yang dilakukan springbbot repon di oracle db nya ini.

## Hands - On
Langkah 1 : Buat file ```bash docker-compose.yaml```

```yaml
services:
############ Jenkins ############
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
    privileged: true
    networks:
      - cicd-net
#################################

############ Cloudflared ############
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --url http://jenkins:8080
    networks:
      - cicd-net
    depends_on:
      - jenkins
#####################################

############ SonarQube ############
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    depends_on:
      - db
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    ports:
      - "9001:9000"
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_extensions:/opt/sonarqube/extensions
    networks:
      - cicd-net
###################################

############ PostgresQL ############
  db:
    image: postgres:13
    container_name: sonar-postgres
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - cicd-net
####################################

volumes:
  jenkins_home:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_logs:
  sonarqube_extensions:
  postgresql:
  postgresql_data:

networks:
  cicd-net:
    driver: bridge
services:
############ Jenkins ############
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
    privileged: true
    networks:
      - cicd-net
#################################

############ Cloudflared ############
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --url http://jenkins:8080
    networks:
      - cicd-net
    depends_on:
      - jenkins
#####################################

############ SonarQube ############
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    depends_on:
      - db
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    ports:
      - "9001:9000"
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_extensions:/opt/sonarqube/extensions
    networks:
      - cicd-net
###################################

############ PostgresQL ############
  db:
    image: postgres:13
    container_name: sonar-postgres
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - cicd-net
####################################

volumes:
  jenkins_home:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_logs:
  sonarqube_extensions:
  postgresql:
  postgresql_data:

networks:
  cicd-net:
    driver: bridgeservices:
############ Jenkins ############
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
    privileged: true
    networks:
      - cicd-net
#################################

############ Cloudflared ############
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --url http://jenkins:8080
    networks:
      - cicd-net
    depends_on:
      - jenkins
#####################################

############ SonarQube ############
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    depends_on:
      - db
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    ports:
      - "9001:9000"
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_logs:/opt/sonarqube/logs
      - sonarqube_extensions:/opt/sonarqube/extensions
    networks:
      - cicd-net
###################################

############ PostgresQL ############
  db:
    image: postgres:13
    container_name: sonar-postgres
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - cicd-net
####################################

volumes:
  jenkins_home:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_logs:
  sonarqube_extensions:
  postgresql:
  postgresql_data:

networks:
  cicd-net:
    driver: bridge
```

2. Jalankan docker compose up 

```bash

docker compose up

```

**Setup di sisi SonarQube**

3. Akses SonarQube di ```localhost:9001```

   ***default credentials*** : ```admin/admin```

4. Klik create a local project dan masukkan project display name, project key, dan main branch name; lalu next.
   
![image](https://github.com/user-attachments/assets/5d228bba-adf3-4819-9b13-3c5ea3e727fd)

5. Klik 'Use the global setting' lalu klik 'Create Project'.
   
![image](https://github.com/user-attachments/assets/c4f05477-e105-4b51-8b7a-a98d69373bc8)

6. Klik locally.
   
![image](https://github.com/user-attachments/assets/3f68b462-d9bf-48dc-baa8-315b7c899377)

7. Klik 'Generate' untuk mendapatkan token.

![image](https://github.com/user-attachments/assets/fefee31f-bc5f-4e5e-807a-beb1160fe457)

8. Akan muncul token yang di generate dan klik 'Continue'.

![image](https://github.com/user-attachments/assets/d24df135-6051-4b61-aca0-1c417bfc55f0)

9. Kita akan menjalankan ini di Jenkinsfile

![image](https://github.com/user-attachments/assets/3564840f-56d6-4034-aeab-e93e8fdf951f)

```groovy
pipeline {
  agent any
  tools {
    maven 'Maven 3.8.8'       
    jdk 'Temurin JDK 17'
  }
 
  environment {
    SONARQUBE_SERVER = 'SonarQube' 
  }
 
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/nerissaarvianaputr/cicd', branch: 'main'
      }
    }
 
    stage('Unit Test & Coverage') {
      steps {
        sh 'mvn package'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }
 
    stage('Static Code Analysis (SAST) via Sonar') {
      steps {
        sh """
            mvn clean compile sonar:sonar \
              -Dsonar.projectKey=springboot \
              -Dsonar.projectName='springboot' \
              -Dsonar.host.url=http://sonarqube:9000 \
              -Dsonar.token=sqp_809e51eb2cb024eb5d336fc2df5de05e44f0ee65
        """
      }
    }
  }
 
  post {
    success {
      echo "Pipeline berhasil 🚀"
    }
    failure {
      echo "Pipeline gagal 💥"
    }
  }
}
```

**Setup di sisi Jenkins**

10. Akses Jenkins di ```localhost:8080```.

11. Di dashboard, klik 'New Item'.

12. Konfigurasi New Ittem dengan memilih pipeline (Karena akan ada banyak step yang dilakukan).
    
![image](https://github.com/user-attachments/assets/2c1e3e0a-a165-4782-b3e5-3ca5f4ad4b6a)

14. Untuk Install Maven dan JDK, Klik 'Manage Jenkins', lalu klik 'Tools'.

![image](https://github.com/user-attachments/assets/74fc3206-f756-463a-a644-ffa75e8e93e2) ![image](https://github.com/user-attachments/assets/e6ed62d7-edf9-400a-81aa-cbe4f8344a00)

15. Setelah itu, klik 'Save'.

16. Pada Jenkins, trigger build dengan klik 'Build Now'

![image](https://github.com/user-attachments/assets/f5be889e-925c-4396-a733-4623a1f86d53)

17. Cek di SonarQube akan muncul seperti ini

![image](https://github.com/user-attachments/assets/004c0237-d9e5-44de-8835-be59ea6382ee)







**Jenkins**

pipeline script : Jenkinsfile nya di dalam script di jenkins
pipeline SCM : Jenkinsfile nya di dalam source code di repository










