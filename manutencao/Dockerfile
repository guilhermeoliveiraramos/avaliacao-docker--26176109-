# Página de manutenção do portal, deixada pelo fornecedor anterior.
# Deveria mostrar o aviso "Voltamos em breve" na porta 80 do container.
FROM nginx:1.27-alpine

FROM nginx:1.27-alpine
COPY site/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
