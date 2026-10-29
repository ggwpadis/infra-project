# Infra project

Учебный инфраструктурный репозиторий курса 

<!-- Допишите сюда -->
Учебный инфраструктурный репозиторий: Docker и Ansible.
README updated through GitHub web interface.
## Запуск в Docker

The project includes a personalized Nginx web page.

Build the image:

    docker build -t adis-infra-page ./docker

Run the container:

    docker run -d --name adis-infra-page -p 8080:80 adis-infra-page

Test the page:

    curl http://localhost:8080
