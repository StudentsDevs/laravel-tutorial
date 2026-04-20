Laravel own docs ->understanding (Progress) 

note: update this everytime you add progress

(Laravel herd, laragon, vs code)

-> Laravel set-up
run this command to create new Laravel project (currently using herd and laragon(MySQL database):
	-- laravel new appname
	-- choose desired set-up (i use none on this setup)

-> Set-up and migrate database (laragon) 
	1) should have in .env

	DB_CONNECTION=mysql
	DB_HOST=127.0.0.1
	DB_PORT=3306
	DB_DATABASE=your_db_name
	DB_USERNAME=root
	DB_PASSWORD=
	
	2) then run
	-- php artisan migrate

-> if success
	-- composer run dev (backend and frontend)
	note: if using MySQL should click this port ( http://127.0.0.1:8000 )

-> Controllers and Routes 

	-- to create file
	-- php artisan make:FolderName FileName

	-- common set-up for routes
	//modern routing (must use)
	use App\Http\Controllers\PagesController; --import path
	Route::get('/', [fileName::class, 'functionName']);

	-- Controllers basic set up
	<?php

	namespace App\Http\Controllers;

	use Illuminate\Http\Request;

	class PagesController extends Controller
	{
    	   public function index(){
         	return view('pages.index');
    	   }
	}
	
	-- to check for routes run
	   php artisan route:list

->Layouts
	-- is like a component where you can import and use in other files
	-- create folder inside view named layouts
	-- create a file app.blade.php
	-- inside the file use @yield('content')
	
	EXAMPLE: 
	<!DOCTYPE html>
	<html lang="{{config('app.locale')}}">
	<head>
    	     <meta charset="UTF-8">
    	     <meta name="viewport" content="width=device-width, initial-scale=1.0">
    	     <meta http-equiv="X-UA-Compatible" content="ie=edge">
	     --> use this to import tailwind
	     @vite(['resources/css/app.css', 'resources/js/app.js'])
    	     <title>{{config('app.name', 'Laravel')}}</title>
	</head>
	<body>
    	     @yield('content')
	</body>
	</html>
	
	-- then use it on other files using
	@extends('layout.app);
	-- wrap the content in sections

	@extends('layouts.app')

	@section('content')
    	   <h1>Welcome to Laravel</h1>
	@endsection

-> Passing data from controller to pages
	--initialize a variable
	$title = 'this is a title'
	
	-- then pass it to the page using compact
	-- return view('pages.index', compact('title'));

	--can also use for passing array/multiple values
	return view('pages.index')->with('title', $title);

	--> FOR PASSING ARRAYS
	public function services()
	{
	    $data = array(
            'title' => 'Services',
            'services' => ['Web Dev', 'Mobile Dev', 'DevOps']
        );
        return view('pages.services')->with($data);
    	}

	--then call it on pages
	@extends('layouts.app')

	@section('content')
    	   <h1>{{$title}}</h1>
	{{--check if services has values--}}
    	@if(count($services) > 0)
           <ul>
	     {{--then loop through the array--}}
             @foreach ($services as $service)
                <li>{{$service}}</li>
             @endforeach
           </ul>
        @endif
	@endsection
	
	
-> Adding Navigation bar
	-- create a folder views/includes
	-- create a file navbar.blade.php
	-- on app.blade.php add @include('includes.navbar') on the top of the div of the contents

-> Models and Database Migration
	-- create a PostController 
	-- run this: php artisan make:controller PostController --resource
	-- create a model 
	-- run this: php artisan make:model Post -m
	-- then see databse/migrations/create_post_model_table.php
	-- you can config the model by adding entities on the Schema
	    $table->id();
            $table->string('title');
            $table->mediumText('body');
            $table->timestamps();
	-- import these in Providers/AppServiceProvider.php
	    use Illuminate\Support\Facades\Schema;
	-- then add this inside boot() function
	    Schema::defaultStringLength(191);
	-- then run php artisan migrate

-> Adding data using Tinker
	-- php artisan tinker
	-- create a variable
	   $post = new App\Models\Post
	   = App\Models\Post {#7373} -> if something like this, it worked
	-- use the variable to add data on the table
	   $post->column name = "The data you want to add"
	-- should look like this $post->title = "This is a title"
	-- then save 
	   $post->save()
	-- check database if data are added

-> Adding routes the easy way
	-- instead of initializing routes individually use this
	   //import the path first
	   use App\Http\Controllers\PostController;
	   //then use this for adding all routes from the PostController
	   Route::resource('posts', PostController::class);
	-- then check in terminal/cmd
	   php artisan route:list
	   

-> Getting data from the database using Eloquent
	-- getting all the data of the table
           $posts =  Post::all();
	-- getting data in order
	   $ascOrder = Post::orderBy('title', 'desc')->get();
	-- getting data using where clause
	   $getUsingWhere = Post::where('table_entity_name', 'condition');
-> Using MySQL query
	-- import
	   use Illuminate\Support\Facades\DB;
	   //$posts = DB::select('SELECT * FROM  posts');
	-- Eloquent is much recommended and cleaner
-> Pagination
	-- limits on how many will show per page 
	-- on PostController
	   $posts = Post::orderBy('title', 'desc')->paginate(1);
	-- on posts/index.blade.php add below the @endforeach
	   {{$posts->links()}}
	   
	   