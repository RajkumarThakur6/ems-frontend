I'm new to Docker, I'm trying to dockerize my react application.

This is my Dockerfile:

FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
This is my vite-config file:

import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    host: '0.0.0.0',
    // host: true, Tested with both options
    port: 3000,
  },
})
I am creating the image like this: sudo docker build -t frontend .

I am running the image like this: sudo docker run -it frontend  When I run this command, this is what I get on my terminal

Terminal

When I open both options, Local and Network, I get the same error on my browser.

enter image description here

Can anyone please help me?

Ans-It looks like you're not publishing the container port, it is now only available from inside the docker network. Using the -p-flag, you can publish it to the host machine and you should be able to access vite.

docker run -it -p 3000:3000 frontend
