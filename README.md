## divisione laravel frontend e backend

    - composer create-project laravel/laravel:^9.2 example-app
    - php require laravel/breeze --dev
    - php artisan breeze:install
    - composer require pacificdev/laravel_9_preset
    - php artisan preset:ui bootstrap --auth
    - npm i
    - npm run dev
    - compilare il file .env con le info del DB
    - Lanciare la prima: php artisan migrate
    - Spostiamo la rotta della dashboard all'interno del gruppo gestito dal middleware
    - Sposto la logica della funzione della pagina di dashboard nel controller: php artisan make:controller Admin/AdminController
    - Modificare il file RouteServiceProvider: public const HOME = '/admin';
    - Il gruppo di rotte gestite dal middleware devono essere tutte con il path http che inizia con: localhost:8000/admin..
        - Aggiungere il: ->prefix('nome del prefisso rotta')       Al Gruppo di rotte
        - Modifcare il path delle rotte interno al gruoppo se necessario 
        - Modificare i link associati ai bottoni preesistenti di laravel breeze 
    - Tutte le rotte devono avere anche il name() che inizia con admin.
        - Aggiungere la funzione ->name('admin.') Dopo il ->prefix('admin')


## Se vogliamo usare un campo unique in una colonna e poi abbinare la modifica con crud
     - creare il file Request tramite: php artisan make:request UpdatePostRequest
     - Utilizzare il file al'interno del controller per update e store: public function update(UpdatePostRequest $request, Post $post)
     - Nel file request aggiornare il campo false di authorized in true
     - inseirre nella funzione rule() la validazione per ogni campo colonna da controllare
     - Importare la classe Rule nel file request: use Illuminate\Validation\Rule;
     - Scrivere questa funzione: public function rules()
    {
        return [
            'title' => [ 'required', 'max:20', Rule::unique('posts')->ignore($this->post) ],
            'content' => ['nullable']
        ];
    }


## Upload images
    - modificare il disks storage in config/fylesistems.php e nel .env per renderlo 'public'
    - Lanciamo il comando: php artisan storage:link
    - aggiornare migrations,models ($fillable) con la nuova colonna per le immagini path, e aggiornare la validazione 
    - aggiungee al form l'attributo enctype e il campo input type file con il name della colonna della migations
    - scrivere la logica per il controller nella update e store
    - /* caricamento dell'immagine se presente */
        if( $request->hasFile('cover_image') ){
            
            //PROCEDIMENTO SOLO SE SIAMO NELLA UPDATE!!!!!
            if( $post->cover_image ){
                Storage::delete($post->cover_image);
            }
            //FINE: PROCEDIMENTO SOLO SE SIAMO NELLA UPDATE!!!!!

            //Genere un path di dove verrà salvata l'iimagine nel progetto
            $path = Storage::disk('public')->put( 'post_images', $request->cover_image );

            $form_data['cover_image'] = $path;
        }

## cancellare l'immagine nella storage al cancellamento del records nella tabella
    - if( $post->cover_image ){
                Storage::delete($post->cover_image);
        }   

## Se vogliamo usare lo slug
    - Modificare le rotte aggiungendo ->parameters([ ....Dato parametro diverso da Id...... ])
    - 
## cos'è breeze? 
    - Pacchetto per l'installazione dell'autenticazione con laravel
## cosa fare dopo la clonazione ?
    - composer i
    - npm i
    - cp .env.example .env
    - php artisan key:generate
    - php artisan serve
    - Ricordarsi: di usare: @vite(['resources/js/app.js'])

## Design pattern di Laravel ?
    - MVC : Model view Controller

## cos'è il model ?
Classe che mappa gli attributi e i recrd della tabella di riferiento
## cos'è il controller ?
classe che gestisce il funzionamento tra le nostre richiestre Http e la visualizzazione delle potenziali view
## cos'è la view ?
Sono le nostre pagine scritte in php / blade / vue / react....., in base alla rotta

## cos'è un middleware ? 
Logica di controllo che noi utiliziamo all'occorrenza tra la nostra chiamata Http a una rotta e il raggiungimento finale della logica all'interno del controller: esempio middleware 'Auth'

## cos'è il RouteServiceProvider? 
Gestisce la rotta di atterraggio dopo la login o il register iniziale
