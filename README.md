# BoschTest - below Ansible playbook will deploy to do app and its sql server
- hosts: all
  become: yes
  tasks:

    - name: Create Network
      community.docker.docker_network:
        name: todoapp

    - name: Deploy Wordpress
      community.docker.docker_container:
        name: app
        image: node:12-alpine
        ports:
          - "3000:3000"
        working_dir: /app
        volumes:
          - ./:/app
        environment:
          MYSQL_HOST: mysql
          MYSQL_USER: root
          MYSQL_PASSWORD: secret
          MYSQL_DB: todos
        restart_policy: always


    - name: Deploy MYSQL
      community.docker.docker_container:
        name: db
        image: mysql:5.7
        volumes:
          - todo-mysql-data:/var/lib/mysql
        env:
          MYSQL_DATABASE: "todos"
          MYSQL_ROOT_PASSWORD: 'secrets'
        restart_policy: always
