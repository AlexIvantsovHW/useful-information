# useful-information
<details>
  <summary>Deploy Full stack project</summary>
  youtube link -https://www.youtube.com/watch?v=qPvPvc7aFZg

  Algorithm:
  Some changes in your projects...
Don't forget change domain in cookie settings

//1 - Add new root user
adduser max

open file:
  visudo

Add new row "max ALL=(ALL:ALL)ALL" after "root ALL=(ALL:ALL) ALL"

switch with command "su - tom"
test with command "sudo apt-get update"

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//2 - Install nvm, node, npm

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

nvm -v

(install same version as in your PC)
nvm install node 21.6

test with commands "node -v" or "npm -v"

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//3 - Git pull and push

git add .
git commit -m 'Init'
git add origin
git push origin main


// Create key for git in server

ssh-keygen -o -t rsa -C “ssh@github.com”
"ll" - for test
"cat id_rsa.pub" //copy

paste to github account

mkdir "project-folder"

sudo apt-get install git-all

//change to your git url
git clone git@github.com:username/front.git
git clone git@github.com:username/back.git

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//4 - Database

sudo apt update
sudo apt install postgresql postgresql-contrib

sudo -u postgres psql

// Please change db name and username

CREATE ROLE max WITH LOGIN PASSWORD '123456' CREATEDB;
CREATE DATABASE red_planner OWNER max;
GRANT ALL PRIVILEGES ON DATABASE red_planner TO max;

// list of users
\du

// list of dbs
\l

// Exit
\q

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//5 - Env and install dep

create .env back and front

NODE_ENV = production
DATABASE_URL = postgresql://max:123456@localhost:5432/red_planner?schema=public
JWT_SECRET = FedDrfg&#

// (not reaquired) if you need with pnpm or yarn
npm install -g pnpm
npm install -g yarn

yarn or pnpm install or npm install

// prisma for back
npx prisma db push (only first lunch after use "npx prisma migrate deploy")

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//6 - PM 2 (process manager)

npm run build

npm install pm2 -g

// for npm
pm2 start npm --name client -- start
pm2 start npm --name server -- start

// for yarn
pm2 start yarn --name client -- start
pm2 start yarn --name server -- start

// for pnpm
pm2 start pnpm --name client -- start
pm2 start pnpm --name server -- start


//autolunch
pm2 startup ubuntu

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//7 - Nginx (web-server)

sudo apt-get install nginx

sudo nano /etc/nginx/sites-available/default

server {
  location /api {
        proxy_pass http://localhost:4200;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

location /{
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
		}

//for test
sudo nginx -t

sudo service nginx restart

//if u have statics folders
location /public {
    include /etc/nginx/mime.types;
    root /home/....;
}

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
//8 - Bonus SSL

sudo apt install certbot python3-certbot-nginx
sudo systemctl reload nginx

//change domain
sudo certbot --nginx -d test.com

sudo systemctl status certbot.timer

//check for errors
sudo certbot renew --dry-run

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

If u update files, you should on server:


git pull && pnpm run build && pm2 reload all
	
</details>
<details>
  <summary>Testing UI in Frontend</summary>
  link https://www.youtube.com/watch?v=g3GFZx1KyWs
</details>

<details>
	<summary>Prisma installation and overview</summary>
	link https://www.youtube.com/watch?v=tyCwTTcWcYE
</details>
<details>
	<summary>Nginx</summary>
	link [https://www.youtube.com/watch?v=tyCwTTcWcYE](https://www.youtube.com/watch?v=2aoOEnZmCmQ)
</details>
<details>
  <summary>Prisma</summary>
  ### 🚀 Применение изменений Prisma-схемы на продакшен (VPS)

| Шаг | Действие                                | Команда / Описание                                                        |
|-----|-----------------------------------------|---------------------------------------------------------------------------|
| 1   | Сохрани изменения в `schema.prisma`     | Внеси нужные изменения в `prisma/schema.prisma`                          |
| 2   | Сгенерируй миграцию (локально)          | `npx prisma migrate dev --name имя_миграции`                             |
| 3   | Проверь миграции                        | Убедись, что миграции корректны и всё работает локально                  |
| 4   | Закоммить миграции в git                | `git add prisma/migrations && git commit -m "feat: новая миграция"`     |
| 5   | Отправь код на сервер                   | Через `git push` + SSH или CI/CD                                         |
| 6   | Зайди на VPS                            | `ssh user@your_vps_ip`                                                   |
| 7   | Перейди в папку проекта                 | `cd /path/to/your/project`                                               |
| 8   | Обнови код                              | `git pull`                                                                |
| 9   | Установи зависимости (если нужно)       | `npm install` или `yarn`                                                 |
| 10  | Примени миграции                        | `npx prisma migrate deploy`                                              |
| 11  | Перезапусти приложение                  | Например, `pm2 restart app` или `docker restart`                         |
| 12  | Проверь логи и работоспособность        | `pm2 logs` или `docker logs`, и проверь приложение                       |


### 🛠 Решение проблем с миграциями (например, ошибка P3009)

| Шаг | Действие                                                                 | Команда / Пояснение                                                                 |
|-----|--------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| 1   | Посмотри статус миграций                                                | `npx prisma migrate status`                                                         |
| 2   | Убедись, что действительно есть неудачная миграция                      | В выводе будет `Migration failed`                                                   |
| 3   | Прими решение: откатить или признать успешной                           | Анализируй, были ли изменения применены                                            |
| 4А  | ✅ Частично отработала — признать успешной                               | `npx prisma migrate resolve --applied имя_миграции`                                |
| 4Б  | ❌ Сломала БД — восстановить вручную или удалить                        | Удали вручную изменения в БД или используй `migrate reset` (⚠️ НЕ на продакшене!)  |
| 5   | После исправления — снова запусти миграции                              | `npx prisma migrate deploy`                                                         |

</details>

<details>
	<summary>HTTP codes</summary>
## 📊 HTTP Status Codes Overview

| Код | Название                      | Описание                                                                 |
|-----|-------------------------------|--------------------------------------------------------------------------|
| 200 | OK                            | Запрос выполнен успешно.                                                 |
| 201 | Created                       | Ресурс успешно создан.                                                   |
| 202 | Accepted                      | Запрос принят, но еще не обработан.                                      |
| 204 | No Content                    | Успешно, но без возвращаемого содержимого.                              |
| 400 | Bad Request                   | Неправильные данные запроса (например, ошибка валидации).               |
| 401 | Unauthorized                  | Необходима авторизация.                                                  |
| 403 | Forbidden                     | Доступ запрещён, даже при наличии авторизации.                           |
| 404 | Not Found                     | Ресурс не найден.                                                        |
| 405 | Method Not Allowed            | Метод запроса не поддерживается для данного ресурса.                     |
| 409 | Conflict                      | Конфликт при выполнении запроса (например, дублирование записи).         |
| 422 | Unprocessable Entity          | Семантически неверные или пустые данные (часто валидационные ошибки).   |
| 429 | Too Many Requests             | Слишком много запросов за короткое время.                                |
| 500 | Internal Server Error         | Внутренняя ошибка сервера.                                               |
| 502 | Bad Gateway                   | Сервер получил некорректный ответ от другого сервера.                    |
| 503 | Service Unavailable           | Сервис временно недоступен (например, на обслуживании).                  |
| 504 | Gateway Timeout               | Превышено время ожидания от вышестоящего сервера.                        |

> ℹ️ **Примечание**:
> - `400` и `422` часто используются для сигнализации об ошибках в пользовательском вводе.
> - `204` полезен для запросов, где нет необходимости возвращать контент (например, DELETE).
> - `429` рекомендуется при реализации ограничений на частоту запросов (rate limiting).

</details>
</details>
<details>
  <summary>Docker</summary>
  link https://www.youtube.com/watch?v=g3GFZx1KyWs
  link https://www.youtube.com/watch?v=jYFyLLqvHy8	
</details>
