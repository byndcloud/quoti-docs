# Arquitetura

!!! warning "Documentação descontinuada"
    Esta página faz parte de uma versão descontinuada da documentação. Está presente neste acervo e neste formato enquanto seu conteúdo não é inteiramente migrado para as demais sessões desta documentação.





## Arquitetura da infraestrutura da Plataforma

![Quoti_Architecture.svg](Arquitetura%209c8bf5a2d8164bb197c759d8ef470ad5/Quoti_Architecture.svg)

### Componentes essências:

- Kubernetes
    - Utilizado para servir a aplicação *frontend* e hospedar a **Quoti API**  (em serviços separados)
- MySQL Database
    - Utilizado como banco de dados principal do Quoti Platform
- [Min.io](http://Min.io) Object Storage server
    - Utilizado como servidor de armazenamento de arquivos estáticos (anexos, imagens, extensões, etc.)

### Componentes opcionais:

- Redis Database
    - Utilizado para cache das requisições trafegadas pela **Quoti API**
- Firebase Auth
    - Utilizado para possibilitar a autencação multi-plataforma com Google, Microsoft, Apple, Github, facebook e outros
- Firebase Firestore
    - Utilizado como banco de dados para agilizar o desenvolvimento de extensões
- Vimeo Service Account
    - Utilizado para possibilitar envio e reprodução de videos na plataforma

## Arquitetura da infraestrutura de extensões

![Quoti Architeture-Extensions Architecture.drawio.png](Quoti_Architeture-Extensions_Architecture.drawio.png)
