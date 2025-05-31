Laravel API Documentation

This document provides the steps for building the CRUD Laravel API. Here I have used a posts table which used to store [title and description]. Below is the structure of the project.
1. Project Setup
I installed Laravel in my local machine and created a laravel project named “crud-api”.

    composer create-project laravel/laravel crud-api

Then to install api in the project run below in the terminal.

    php artisan install:api 
2. Configure Database
In the .env file, I have updated the details as per my local database connection. Here I used a mysql database. 

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=Raj@1234

Then run below given command in the terminal:

    php artisan migrate
3. Create Model, Migration, and Controller:

Run this below given command in the terminal which helps to generate a model, migration and controller files.
 
    php artisan make:model Post -mc

-m	It Also create a migration file for the posts table
-c	It Also create a controller for the model (PostController)



4. Migration File:
In the Migration file, I have defined the structure of the database table (posts). And added two fields in the schema (title, description).

Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->text('description');
    $table->timestamps();
});

Then Run migration:
    php artisan migrate

5. Model File:
This line in the Post model defines which fields are allowed to be mass-assigned. Mass assignment is a convenient way to create or update a model using an array of key-value pairs.

Added below given line in the Model file (Post.php).

protected $fillable = ['title', 'description'];

6. API Routes:

In the`routes/api.php` file define the api routes for the “Post” model. 

routes/api.php file:
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\PostController;

Route::get('/posts', [PostController::class, 'index']);

Route::get('/posts/{id}', [PostController::class, 'show']);

Route::post('/posts', [PostController::class, 'store']);

Route::put('/posts/{id}', [PostController::class, 'update']);

Route::delete('/posts/{id}', [PostController::class, 'destroy']);

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');

7. Controller File:
In the controller file, I have defined the logic for handling api requests of the “Post” model.

`app/Http/Controllers/PostController.php file`:

<?php

namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    public function index() {
        return Post::all();
    }

    public function store(Request $request) {
        $post = Post::create($request->only(['title', 'description']));
        return response()->json($post, 201);
    }

    public function show(Post $post) {
        return $post;
    }

    public function update(Request $request, Post $post) {
        $post->update($request->only(['title', 'description']));
        return response()->json($post);
    }

    public function destroy(Post $post)
{
    $post->delete();
    return response()->json(['message' => 'Post deleted successfully'], 200);
}
}



8. API Endpoints

Method
Endpoint
Description
GET
/api/posts
List all posts
GET BY ID
/api/posts/{id}
Get a specific post by id
POST
/api/posts
Create a post
PUT
/api/posts/{id}
Update a post by id
DELETE
/api/posts/{id}
Delete a post by id



