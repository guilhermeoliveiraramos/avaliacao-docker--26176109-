# Respostas da Avaliação Prática · ViaSerra Transportes

**Aluno:** Guilherme Oliveira Ramos
**Matrícula:** 26176109

---

### Parte 1 · Dockerfile do portal

**Pergunta 1: Por que é necessário indicar a tag da imagem base (ex: `nginx:1.27-alpine`) em vez de usar `latest` ou nenhuma tag?**
> O uso de tags fixas garante a reprodutibilidade do ambiente de produção. Se utilizarmos `latest` ou nenhuma tag, o Docker descarrega a versão mais recente disponível na altura da compilação, o que pode introduzir quebras de compatibilidade, falhas de segurança inesperadas ou comportamentos inconsistentes em máquinas diferentes.

**Pergunta 2: Por que a instrução `EXPOSE` no Dockerfile não é suficiente para expor a porta do container para a sua máquina?**
> A instrução `EXPOSE` funciona apenas como documentação técnica dentro da imagem (metadados), indicando em qual porta a aplicação escuta internamente. Ela não cria o redirecionamento de rede no sistema operativo hospedeiro. Para expor a porta para a máquina real, é necessário mapeá-la explicitamente na execução com o parâmetro `-p <porta_host>:<porta_container>` ou no `docker-compose.yml`.

---

### Parte 2 · Publicação no Docker Hub

**Pergunta 3: Qual é o nome completo e a tag da imagem que você publicou no Docker Hub?**
> `guilhermeoliveiraramos/viaserra-portal:1.0-26176109`

**Pergunta 4: O que aconteceria na correção se o repositório no Docker Hub estivesse marcado como privado?**
> O professor (ou o verificador automático) não conseguiria descarregar a imagem (`docker pull`) sem estar autenticado com as credenciais da conta proprietária, resultando em erro de acesso negado (*access denied*) e nota zero nesta etapa.

---

### Parte 3 · A página de manutenção

**Pergunta 5: Tabela de defeitos encontrados no Dockerfile da página de manutenção**

| Defeito | O que aconteceu ao rodar | Como foi corrigido |
| :--- | :--- | :--- |
| **Defeito 1: `WORKDIR` incorreto** | Apontava para `/usr/share/nginx` em vez do diretório do site `/usr/share/nginx/html`. | Removido o `WORKDIR` incorreto e ajustado o destino no comando de cópia. |
| **Defeito 2: Ausência do `COPY`** | Os ficheiros de `manutencao/site/` não eram copiados para dentro da imagem. | Adicionada a instrução `COPY site/ /usr/share/nginx/html/`. |
| **Defeito 3: Container encerra ao iniciar (`Exited 0`)** | Faltava o comando para manter o processo do Nginx ativo em primeiro plano. | Adicionada a instrução `CMD ["nginx", "-g", "daemon off;"]`. |

**Pergunta 6: Por que a ordem das instruções no Dockerfile altera o aproveitamento do cache durante o `docker build`?**
> O Docker executa o `build` em camadas ordenadas de cima para baixo. Se uma instrução for alterada, todas as camadas seguintes têm o cache invalidado e precisam de ser reconstruídas. Por isso, instruções que mudam com pouca frequência (como dependências e instalações) devem ficar no topo, enquanto ficheiros de código e configurações que mudam frequentemente devem ficar no final.

---

### Parte 4 · Primeiro docker-compose

**Pergunta 7: Qual é a vantagem de utilizar o `docker-compose.yml` em vez de executar múltiplos comandos `docker run` manualmente?**
> O Docker Compose permite definir toda a infraestrutura de múltiplos containers de forma declarativa num único ficheiro de configuração. Isso evita ter de memorizar ou digitar comandos extensos e suscetíveis a erros no terminal, garantindo a orquestração e inicialização padronizada de toda a aplicação com um simples `docker compose up -d`.

**Pergunta 8: O que acontece com os containers criados pelo Compose se o arquivo `docker-compose.yml` for alterado e o comando `docker compose up -d` for executado novamente?**
> O Docker Compose analisa as diferenças entre o ficheiro atualizado e os containers em execução. Ele recria apenas os serviços cujas configurações ou imagens sofreram alterações, mantendo os restantes containers intactos e sem interrupções desnecessárias.

---

### Parte 5 · Verificador e Entrega

**Pergunta 9: Cole aqui o código de conclusão gerado pelo verificador:**
> *(Substitua este texto pelo código único exibido no terminal após rodar o verificador)*