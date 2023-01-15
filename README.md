## Modelo para Projetos com Tenancy - Multi Database

__Iniciando Projeto__

* Insatalando o Laravel usando Sail + Mysql + Redis

        curl -s "https://laravel.build/example-app?with=mysql,redis" | bash

> Sail é uma ferramenta de desenvolvimento de back-end para Laravel, que fornece uma maneira fácil de criar e gerenciar aplicativos de API e de banco de dados. Laravel é um framework PHP popular para desenvolvimento web. MYSQL é um sistema de gerenciamento de banco de dados relacional (RDBMS) e Redis é um banco de dados NoSQL com suporte a chave-valor. Juntos, essas ferramentas podem ser usadas para criar aplicativos web completos, incluindo a camada de banco de dados, gerenciamento de sessão, cache e muito mais.

* Confingurar o Sail e subir o container

        cd example-app

        alias sail="./vendor/bin/sail up"

        sail ip -d

__Configurando Pacote__

* Para baixar o pacote tenacy

        sail composer require stancl/tenancy

* Instalar o pacote tenacy no projeto

        sail artisan tenancy:install

* Para habilitar o tenancy no laravel vá em confing/app.php no array de 'providers'=>[ após RouterServiceProvider];

        App\Providers\TenancyServiceProvider::class

__Adequando Models__

* Gerar os Models 

        sail artisan make:model Tenant
        sail artisan make:model Domain

> Pode mover use Stancl\Tenancy\Database\Models\Tenant; de config/tenancy.php para app/Models/Tenant.php.
> Pode Remover use Illuminate\Database\Eloquent\Model;

        Stancl\Tenancy\Database\Models\Tenant as BeseTenant;

> Pode mover use Stancl\Tenancy\Database\Models\Domain; de config/tenancy.php para app/Models/Domain.php.
> Pode Remover use Illuminate\Database\Eloquent\Model;

        Stancl\Tenancy\Database\Models\Domain as BeseDomain;

> Em config/tenancy.php deve fazer a importação do model

    use App\Models\{Tenant, Domain};

__Adequando Domínios Centrais__

* Abrar app/Providers/RouteServiceProvider.php substitua

        public function boot()
        {
                $this->configureRateLimiting();

                $this->routes(function () {
                        $this->mapWebRoutes();

                        $this->mapApiRoutes();
                });
        }

> Logo a abaixo adicione esse codigo

        public function mapApiRoutes(){
            foreach($this->centralDomains() as $domain){
                Route::middleware('api')
                        ->domain($domain)
                        ->prefix('api')
                        ->group(base_path('routes/api.php'));
            }
        }

        public function mapWebRoutes(){
            foreach($this->centralDomains() as $domain){
                Route::middleware('web')
                        ->domain($domain)
                        ->group(base_path('routes/web.php'));
            }
        }

        protected function centralDomains()`
        {
            return config('tenancy.central_domains');
        }

__Criando Migrações Tenant__

        sail artisan make:migration create_*NAME*s_table --path=database/migrations/tenant