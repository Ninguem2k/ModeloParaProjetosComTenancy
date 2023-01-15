## Modelo para Projetos com Tenancy - Multi Database

__Iniciando Projeto__

* Insatalando o Laravel usando Sail + Mysql + Redis

    curl -s "https://laravel.build/example-app?with=mysql,redis" | bash

> Sail é uma ferramenta de desenvolvimento de back-end para Laravel, que fornece uma maneira fácil de criar e gerenciar aplicativos de API e de banco de dados. Laravel é um framework PHP popular para desenvolvimento web. MYSQL é um sistema de gerenciamento de banco de dados relacional (RDBMS) e Redis é um banco de dados NoSQL com suporte a chave-valor. Juntos, essas ferramentas podem ser usadas para criar aplicativos web completos, incluindo a camada de banco de dados, gerenciamento de sessão, cache e muito mais.

* Confingurar o Sail e subir o container

    cd example-app

    alias sail="./vendor/bin/sail up"

    sail ip -d

