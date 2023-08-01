<div align="center">

# Sistema de Gerenciamento de Tarefas (API)

### Esse repositório contém uma API solicitada para prática e avaliação da matéria de PHP e API's.

</div>

</br>

**API em execução no Postman:**

[![](https://markdown-videos.vercel.app/youtube/V7IgQjuNsrU)](https://www.youtube.com/watch?v=V7IgQjuNsrU)

**Instruções**

- ``` git clone ``` (link do repositório)
- ``` composer install ``` (execute no terminal para baixar as dependências do projeto)
- Inicie no XAMPP o Apache e o MySQL
-  ``` php artisan migrate  ```
- ``` php artisan serve ``` (execute no terminal para iniciar o servidor)

----

**Postman**

- Criar uma nova collection
- Adicionar 5 requests
 1. Lista todas tarefas - Get
 2. Detalhes de tarefa específica - Get
 3. Adiciona nova tarefa - Post
 4. Atualiza dados da tarefa - Put
 5. Exclui tarefa - Del

![requisições postman](https://github.com/p1nheiros/Sistema-de-Gerenciamento-de-Tarefas-API/assets/124714182/2e1da62f-ce84-429a-91c1-3178fba017af)

- Insira o URL que foi gerado no comando _php artisan serve_
 1. Get - URL/tasks
 2. Get - URL/tasks/{id}
 3. Post - URL/tasks
 4. Put - URL/tasks/{id}
 5. Del - URL/tasks/{id}

- No método Post e Put vá em ***body***, selecione ***row*** e coloque em JSON

![método postman](https://github.com/p1nheiros/Sistema-de-Gerenciamento-de-Tarefas-API/assets/124714182/025ed5ae-b10f-411a-b418-aeed233a11aa)

```
{
    "title": "Titulo da tarefa",
    "description": "Descrição da tarefa",
    "status": true
}
```

- Pronto, agora é só testar a API.
  
----

### Tutorial

[![](https://markdown-videos.vercel.app/youtube/B1JWzrHdaCI)](https://www.youtube.com/watch?v=B1JWzrHdaCI)

<details>
  <summary>app/Http/Controllers/TaskController.php</summary>
  <br/>
 
 ```
 <?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Task;

class TaskController extends Controller
{
    public function index()
    {
        $tasks = Task::all();

        return response()->json($tasks);
    }

    public function show($id)
    {
        $task = Task::find($id);

        if ($task) {
            return response()->json($task);
        } else {
            return response()->json(['error' => 'Tarefa não encontrada'], 404);
        }
    }

    public function store(Request $request)
    {
        $request->validate([
            'title' => 'required',
            'description' => 'required',
        ]);

        $task = new Task;
        $task->title = $request->input('title');
        $task->description = $request->input('description');
        $task->status = $request->input('status', false);
        $task->save();

        return response()->json($task, 201);
    }

    public function update(Request $request, $id)
    {
        $task = Task::find($id);

        if ($task) {
            $request->validate([
                'title' => 'required',
                'description' => 'required',
            ]);

            $task->title = $request->input('title');
            $task->description = $request->input('description');
            $task->status = $request->input('status', $task->status);
            $task->save();

            return response()->json($task);
        } else {
            return response()->json(['error' => 'Tarefa não encontrada'], 404);
        }
    }

    public function destroy($id)
    {
        $task = Task::find($id);

        if ($task) {
            $task->delete();

            return response()->json(['message' => 'Tarefa excluída com sucesso']);
        } else {
            return response()->json(['error' => 'Tarefa não encontrada'], 404);
        }
    }
}
 ```

  </div>
</details> 

<details>
  <summary>database/migrations/2023_06_27_221132_create_tasks_table.php</summary>
  <br/>
 
 ```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateTasksTable extends Migration
{
    public function up()
    {
        Schema::create('tasks', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description');
            $table->boolean('status')->default(false);
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('tasks');
    }
}
 ```

  </div>
</details> 

<details>
  <summary>routes/api.php</summary>
  <br/>
 
 ```
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/tasks', 'TaskController@index');
Route::get('/tasks/{id}', 'TaskController@show');
Route::post('/tasks', 'TaskController@store');
Route::put('/tasks/{id}', 'TaskController@update');
Route::delete('/tasks/{id}', 'TaskController@destroy');
 ```

  </div>
</details> 


<details>
  <summary>routes/web.php</summary>
  <br/>
 
 ```
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\TaskController;

Route::get('/tasks', [TaskController::class, 'index']);
Route::get('/tasks/{id}', [TaskController::class, 'show']);
Route::post('/tasks', [TaskController::class, 'store']);
Route::put('/tasks/{id}', [TaskController::class, 'update']);
Route::delete('/tasks/{id}', [TaskController::class, 'destroy']);



 ```

  </div>
</details> 

----

<details>
  <summary>👈 Tarefa proposta</summary>
  <br/>

  **Enunciado: Sistema de Gerenciamento de Tarefas (API)**
<p align="left"> 
 
Você foi contratado para desenvolver um sistema de gerenciamento de tarefas utilizando Laravel. O sistema deve fornecer uma API para realizar operações básicas de criação, leitura, atualização e exclusão (CRUD) de tarefas.

**Requisitos:**

1. A API deve ser capaz de lidar com as seguintes operações:
   - Listar todas as tarefas
   - Obter detalhes de uma tarefa específica
   - Criar uma nova tarefa
   - Atualizar os dados de uma tarefa existente
   - Excluir uma tarefa

2. Cada tarefa deve conter as seguintes informações:
   - Título (obrigatório)
   - Descrição (obrigatório)
   - Status (concluída ou não concluída)

3. A API deve seguir as práticas recomendadas para design de APIs RESTful e utilizar os métodos HTTP adequados para cada operação.

4. A resposta da API deve estar no formato JSON.

5. Implemente validações adequadas para garantir que os dados fornecidos estejam corretos antes de executar as operações no banco de dados.

6. Teste a API utilizando uma ferramenta de testes de API, como o Postman ou o Insomnia.

7. Documente a API, incluindo informações sobre as rotas disponíveis, os parâmetros necessários e as respostas esperadas.

**Dicas:**

- Use os recursos do Laravel, como rotas, controladores e modelos, para facilitar o desenvolvimento da API.
- Utilize o sistema de migração do Laravel para criar a tabela de tarefas no banco de dados.
- Crie um controlador dedicado para manipular as operações relacionadas às tarefas.
- Utilize o Eloquent ORM para interagir com o banco de dados.
- Lembre-se de adicionar as validações necessárias nos controladores para garantir a integridade dos dados.

**Bônus (opcional):**

- Implemente recursos de paginação e ordenação para a listagem de tarefas.
- Adicione autenticação na API para garantir que apenas usuários autenticados possam acessar as operações de criação, atualização e exclusão de tarefas.
- Implemente recursos de busca para permitir que os usuários pesquisem tarefas por título, descrição ou status.

**Observação:** Esta atividade foca na criação da API, portanto, não é necessário incluir interface HTML. O objetivo é praticar a criação de endpoints e a manipulação dos dados utilizando o Laravel.

----


## Baixando as Dependências de um Projeto Laravel

Ao clonar um projeto Laravel de um repositório Git, é necessário baixar as dependências do projeto para garantir que todas as bibliotecas e pacotes necessários estejam instalados corretamente. Para isso, siga as etapas abaixo:

1. Certifique-se de ter o PHP instalado em sua máquina. Recomenda-se usar a versão suportada pelo projeto Laravel. Você pode verificar a versão suportada no arquivo `composer.json` do projeto.

2. Abra um terminal e navegue até o diretório raiz do projeto Laravel clonado.

3. Execute o comando `composer install` para baixar as dependências do projeto. O Composer irá ler o arquivo `composer.json` do projeto e instalar todas as dependências listadas.

4. Aguarde até que o Composer conclua o processo de instalação das dependências. Isso pode levar alguns minutos, dependendo da velocidade da sua conexão com a internet.

5. Após a conclusão, o Composer terá criado um diretório chamado `vendor` no diretório raiz do projeto. Esse diretório contém todas as dependências do projeto Laravel.

6. Se houver arquivos de ambiente (`.env`) no projeto, copie o arquivo `.env.example` para `.env` e configure as variáveis de ambiente necessárias, como informações do banco de dados e chaves de acesso.

7. Agora você está pronto para executar o projeto Laravel. Use um servidor web local (por exemplo, o servidor embutido do PHP ou um servidor como o Apache ou Nginx) e acesse o projeto em seu navegador.

Com essas etapas, você poderá baixar e configurar corretamente as dependências do projeto Laravel ao cloná-lo de um repositório Git.

Lembre-se de consultar a documentação oficial do Laravel para obter mais informações sobre a configuração e execução de projetos Laravel.

</p>
  
  </div>
</details> 

<details>
  <summary>✍️ Link do repositório da tarefa</summary>
  <br/>

  **https://github.com/joaovcandrade/projeto-laravel**

  </div>
</details> 
