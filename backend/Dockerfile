FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . . /app/

EXPOSE 80

# Default value if not set
ENV MONGODB_USERNAME=root
ENV MONGODB_PASSWORD=secret

CMD ["npm", "start"]

