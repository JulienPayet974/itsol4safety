FROM node:20-alpine

WORKDIR /app

# Copie les fichiers nécessaires
COPY package*.json ./
RUN npm ci

# Copie tout ton code
COPY . .

# On donne l'adresse du Drupal de démo (pour que ça s'affiche)
ENV NEXT_PUBLIC_DRUPAL_BASE_URL=https://next-drupal-starterkit.dev.wdr.io
ENV DRUPAL_BASE_URL_INTERNAL_IMAGES=https://next-drupal-starterkit.dev.wdr.io
ENV NODE_ENV=production
ENV PORT=3100
ENV CIRCLECI=1

# Build du site Next
RUN npm run build

EXPOSE 3100

# Lancement du serveur
CMD ["npm", "run", "start"]
