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

**SonarQube**

Setup di sisi SonarQube

1. Klik create a local project dan masukkan project display name, project key, dan main branch name; lalu next.
   
![image](https://github.com/user-attachments/assets/5d228bba-adf3-4819-9b13-3c5ea3e727fd)

2. Klik 'Use the global setting' lalu klik 'Create Project'.
   
![image](https://github.com/user-attachments/assets/c4f05477-e105-4b51-8b7a-a98d69373bc8)

4. Klik locally.
   
![image](https://github.com/user-attachments/assets/3f68b462-d9bf-48dc-baa8-315b7c899377)

5. Klik 'Generate' untuk mendapatkan token

![image](https://github.com/user-attachments/assets/fefee31f-bc5f-4e5e-807a-beb1160fe457)

6. Masukkan Jenkinsfile ke dalam VSCode




**Jenkins**

pipeline script : Jenkinsfile nya di dalam script di jenkins
pipeline SCM : Jenkinsfile nya di dalam source code di repository










