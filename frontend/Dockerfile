# -------------------------
# Étape 1 : Builder le frontend
# -------------------------
FROM node:18-alpine AS builder

WORKDIR /app

# Copier les fichiers package pour installer les dépendances
COPY package*.json ./
RUN npm ci

# Copier le reste du projet
COPY . .

# Donner les permissions aux binaires npm (utile parfois)
RUN chmod -R 755 node_modules/.bin

# Build du frontend (Angular / React / Vue)
RUN npm run build

# -------------------------
# Étape 2 : Serveur nginx
# -------------------------
FROM nginx:alpine

# Copier le build dans le dossier de nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# Exposer le port 80
EXPOSE 80

# Optional: HEALTHCHECK simple avec wget (installer wget)
RUN apk add --no-cache wget
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget --no-verbose --tries=1 --spider http://localhost:80 || exit 1

# Lancer nginx en premier plan
CMD ["nginx", "-g", "daemon off;"]
