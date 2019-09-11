# 01 - introduction

![alt text](https://i.imgsonic.com/demo/?src=https://i.imgsonic.com/demo/?src=https://foorious.com/wp-content/uploads/2014/11/Unicorn_Portrait_Black_and_White-unicorn15.png&width=215 "Foorious")

Foorious, inc. is a software company based in Wrocław, Poland.


![alt text](https://i.imgsonic.com/demo/?src=https://i.imgur.com/nTpUdtP.png&width=150 "Colibri")

Colibri--which should in theory be written as "Colibrì"--is a team of software developers based in Wrocław, Poland and managed by Foorious, inc., which focuses exclusively on projects and software-related challenges of [Librì progetti educativi](https://www.progettieducativi.it), a publishing/advertising company based in Florence, Italy.

## About this wiki

This wiki includes guidelines to be followed in all projects of both Foorious and Colibri. It also includes details about workflow, tech stack, project implementations etc.

Follow these instructions so that your work can pass QA for Foorious/Colibri projects.

# 2. Tech stack overview

We use an API-first approach, where as much of the action as possible happens on the front-end, which communicates with the back-end via REST APIs.

On the back-end we use the LAMP stack, on the front-end ReactJS.

We build a lot of projects that have a short lifespan (web apps for events etc.), and we love to take the opportunity to experiment with new technologies or use stuff that might be gone after 6 months.

## 2.1 Back-end

We use the LAMP stack for custom apps and complicated websites, and WordPress on LAMP for simple websites, landing pages, etc.

Reasons to favor LAMP over more trendy technologies (like MEAN and others) are:

* ubiquity
* most people know and have worked in the ecosystem extensively, it's easier to find people to help us out
* lots of very stable tools
* lots of very solid, proven libraries
* WordPress is written in PHP, we can share code if we use WordPress on some projects

### HTTP server
* Apache

### PHP
* Modern PHP (version version 7+ with no regard to previous versions, written following PSRs)
* Composer
* Laravel
* code style: PSR-2 (http://www.php-fig.org/psr/psr-2/), + vars holding primitive values MUST be `$snake_case`, vars that hold objects `$myCamelCase`.

### DB
* MySQL 5.7

## 2.2 Front-end

While our needs are definitely a lot simpler than Facebook's ReactJS seems to be the best tool at the moment:

- everyone is using React, so lots of libraries, developers, etc.
- lots of projects built with React, even if it died tomorrow someone will maintain it for at least 5 years given the investment that many company made
- it's pretty great!

On the data side, we're using REST APIs pretty much exclusively.

### HTML
* HTML 5
* code style following @mdo's style guide (http://codeguide.co/#html)

### CSS
* CSS3
* BEM
* SASS
* code style following @mdo's style guide (http://codeguide.co/#css)
* all IDs and classes MUST be prefixed with the project name ("ex., `.imparato-header`, NOT `.header`). More typing, but easier to refactor and you should type fast so that shouldn't be a problem

### JavaScript
* ES7
* vanilla JavaScript (no jQuery)
* AirBnB code style (https://github.com/airbnb/javascript, https://github.com/mjurczyk/javascript for Polish translation)

### React

* Latest version
* React Router

## 2.3 Version control

For versioning, we use Git via GitHub EXCLUSIVELY.

All repos will have the labels outlined in https://blog.adam-marsden.co.uk/better-github-labels-f1360b43e0a7. Any additional label will follow that convention.

## 2.4 Devops

As of 07/2018, we're using:

* dedicated VPSes on Vultr or own SSD servers
* latest Ubuntu LTS release (using Bionic as of 08/2018)
* Apache
* MySQL 5.7
* GlusterFS + Galera in multi-server configs, using AWS for DNS management and health checks

While developing, we use different technologies according to what is being developed:

- for static websites, we use a Gulp-based setup with hot reloading
- if we need is a simple web server (for instance, for WordPress websites using Sqlite), we use PHP's built-in server
- if developing a Laravel REST API, we use Docker exclusively, and you MUST use https://github.com/nkkollaw/foorious-laravel-api/

See the appropriate section for more details.

# 3. HTML

Coming soon

# 4. CSS

## 4.1 Naming

We use BEM: http://getbem.com/naming/.

In addition, you can add a project-specific namespace. For instance, from:

```css
.card__title--hover {
...
}
```

to:

```css
.bankitalia-card__title--hover {
...
}
```

This can and should be implemented in 1 line using a CSS preprocessor.

## 4.2 Declaration order

We use mdo's style guide: http://codeguide.co/#css-declaration-order/

# 5. JavaScript

Coming soon

# 6. Static websites

Sites that might have some functionality but are mainly static.

If you need PHP, you MUST use https://github.com/foorious-inc/docker-lamp/.

# 7. WordPress websites

For WordPress projects, we use this approach while developing: https://www.paulpepper.com/blog/2017/02/wordpress-development-environment/.

It features a SQLite database instead of MySQL, which dramatically eases sharing changes to the site since the DB can be pushed to the repo.

NOTE that this is for development purposes only. An All-in-one Migration or a manual migration will be performed to trasfer the site to production, and that should use MySQL.

While developing, you MUST using this: https://github.com/foorious-inc/docker-lamp/.

# 8. Web apps

See subtopics.

# 8.1 REST API

All REST APIs for Foorious/Colibri are JSON-based (although they can ALSO return XML and other formats in certain cases), implemented with the Laravel framework.

## 8.1.1 Setup

### LAMP environment

While developing a PHP app/API, you MUST clone https://github.com/foorious-inc/docker-lamp/ for LAMP server and start off that. This is done like so:

#### 1/3 create a repo
Create a repo for your new project. Get repo URL.

#### 2/3 duplicate `docker-lamp` into your repo
```bash
cd /tmp/
git clone --bare https://github.com/foorious-inc/docker-lamp.git foorious-docker-lamp
cd /tmp/foorious-docker-lamp/
git push --mirror your-repo-url # <-- your repo URL here, ex. https://github.com/your-username/your-repo-name.git
rm -v -r -f /tmp/foorious-docker-lamp/
```

#### 3/3 clone your repo, it will contain `docker-lamp`.

### Laravel

#### Add files
Add the contents of https://github.com/nkkollaw/foorious-laravel-api/ to the `www` folder inside your project.

```bash
cd ./public_html/ # <-- go inside your project's www folder, wherever it is
rm -v index.php # if you haven't touched anything, there is an index.php file for testing, which we don't need
git clone https://github.com/nkkollaw/foorious-laravel-api.git
cp -v -r foorious-laravel-api/* .
cp -v -r foorious-laravel-api/.env.example .
rm -v -r -f ./foorious-laravel-api/
```

#### Setup Laravel
Start Docker and enter the container then see https://github.com/nkkollaw/foorious-laravel-api/ for instructions.

## 8.1.2 API guidelines

### 8.1.2.1 Swagger

APIs MUST be documented with OpenAPI (Swagger) version 3 (see https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.1.md). A good editor for OpenAPI definitions is http://editor.swagger.io/.

Plan is to **GENERATE EVERYTHING FROM SWAGGER DEFINITIONS!!**

![](https://i.imgsonic.com/demo/?src=https://media.giphy.com/media/10jFq0frYnX9PG/giphy.gif&width=80)

#### Writing documentation

When you write code, add OAI stuff directly in the code. This, because keeping separate files means we'll forget, and never do it.

Follow the pet store example for how and where to put stuff: https://github.com/zircote/swagger-php/tree/master/Examples/petstore-3.0/.

#### Generate documentation

We then use https://github.com/DarkaOnLine/L5-Swagger/ to parse the PHP files and generate documentation, using `L5_SWAGGER_GENERATE_ALWAYS` so that we don't have to worry about it.

### 8.1.2.2 General guidelines

APIs MUST adhere to the following:

- always start URL with `/api/v1` (or whatever version we're at)
- use nouns for endpoints, never verbs
- GET method and query parameters MUST NEVER alter the state
- ALWAYS use plurar nouns
- use sub-resources for relations
- no UUIDs anywhere, please. Numbers are easier for humans (and possibly machines, too)

CORS MUST be enabled via PHP rather than .htaccess. This makes it easier to test APIs locally since we're not relying on `mod-header` or other server addons.

CORS should already be available in our Laravel boilerplate, but they could be easily implemented with some simple code like:

```php
// array holding allowed origin domains. can be '*' for all, or array for specific domains
$allowed_origins = '*'; /* array(
    '(http(s)://)?(www\.)?my\-domain\.com'
);*/
$allow = false;
if (isset($_SERVER['HTTP_ORIGIN']) && $_SERVER['HTTP_ORIGIN'] != '') {
    if (is_array($allowed_origins)) {
        foreach ($allowed_origins as $allowed_origin) {
            if (preg_match('#' . $allowed_origin . '#', $_SERVER['HTTP_ORIGIN'])) {
                $allow = true;
                break;
            }
        }
    } else {
        if ($allowed_origins == '*') {
            $allow = true;
        }
    }
}
if ($allow) {
    header('Access-Control-Allow-Origin: ' . $_SERVER['HTTP_ORIGIN']);
    header('Access-Control-Allow-Methods: GET, PUT, POST, DELETE, OPTIONS');
    header('Access-Control-Max-Age: 1000');
    header('Access-Control-Allow-Headers: Content-Type, Authorization, X-Requested-With');
}
```

### 8.1.2.3 Endpoints

APIs MUST adhere to the following:

- should follow generic API guidelines (see above)
- always send the correct status code
- along with the status code we always add an envelope with:
  - a boolean `ok`
  - a container for items called `data` (see examples below)
  - int `total_count`, with the total number of items regardless of pagination
  - a *currently* unspecified property `error` + error-related properties (should probably be 1 single object)
- `format` GET parameter to specify format, defaults to `JSON` (could be `XML`, `CSV`, etc., case-insensitive)
- GET endpoints MUST also implement:
  - sorting via `sort_by` for the field, `sort_dir` (`ASC` or `DESC`, case-insensitive) for sorting direction
  - pagination via `offset` and `limit`. `limit` can be `-1` to return all records. Default should be something like 20 or 25.

Sample response for CREATE (notice that we return the whole newly-created object)

```json
{
  "ok": true,
  "data": {
    "pet_id": 1,
    "pet": {...}
  }
}
```

Sample response for UPDATE (notice that we return the whole object again):

```json
{
  "ok": true,
  "data": {
    "pet": {...}
  }
}
```

Sample response for LIST:

```json
{
  "ok": true,
  "total_count": 500,
  "data": {
    "pets": [
      {...},
      {...}
    ]
  }
}
```

Sample response for GET:

```json
{
  "ok": true,
  "data": {
    "pet": {...}
  }
}
```

Sample response for DELETE:

```json
{
  "ok": true
}
```

Sample response for ERROR (very much WIP):

```json
{
  "ok": false,
  "error": "unable to add pet", // generic error about operation
  "error_code": "A45",
  "error_details": [
    {
        "property": "name",
        "message": "name is required"
    },
    {
        "property": "age",
        "message": "age should be a number"
    }
  ]
}
```

#### Naming

If an endpoint has multiple words (ex., "product groups"), we use hyphens (see https://stackoverflow.com/a/18450653/2178206): `/api/v1/product-groups`.

## 8.1.3 Laravel guidelines

- use models for defining and holding data ONLY
- use services with POPOs (plain old PHP objects) for retrieving and saving data, do validation, etc. (NOTE: validation is done inside services instead of controllers, because certain objects could span multiple models)
- routes are mapped to controllers
- controllers call services, and return models. do NOT put any business logic in controllers, EVER

### 8.1.3.1 Models

Models MUST use singular noun, ex.: `\App\Models\Pet`.

Models should contain properties and validation rules. For instance:

```bash
class Pet extends Model
{
    protected $fillable = [
        'name',
        'age'
    ];

    public static function rules()
    {
        return [
          'name' => 'required|max:255',
          'age' => 'required'
        ];
    }
}
```

Models MUST go inside `/app/Models`--you should create this directory if it's not there already.

### 8.1.3.2 Controllers

Controllers redirect an API request to the service manager, and return it to the API (see below for example code).

Classes for controllers MUST be named by pluralizing the model name and appending the word "Controller". For instance: `\App\Controllers\PetsController`.

Method naming for CRUD operations is the following:

- `show`/`list` -- for retrieving 1 or more objects, respectively
- `create` -- for creating
- `update` -- for updating
- `delete` -- to delete objects

Other methods:
- `output` -- for outputting something, like a picture as base64
Here is an example:

```php
class PetsController extends Controller
{
    public function __construct()
    {

    }

    public function list(Request $request)
    {
        try {
            $pets = PetsManager::getPets();

            return response([
                'ok' => true,
                'total_count' => is_array($pets) ? count($pets) : 0,

                'data' => [
                    'pets' => $pets
                ]
            ])->setStatusCode(200);
        } catch (\Exception $e) {
            return response([
                'ok' => false,

                'error' => $e->getMessage()
            ])->setStatusCode(501);
        }
    }

    public function show(Request $request, $pet_id)
    {
        try {
            $myPet = PetsManager::getPetById($pet_id);

            return response([
                'ok' => true,

                'data' => [
                    'pet' => $myPet
                ]
            ])->setStatusCode(200);
        } catch (\Exception $e) {
            return response([
                'ok' => false,

                'error' => $e->getMessage()
            ])->setStatusCode(500);
        }
    }

    public function create(Request $request)
    {
        try {
            $payload = $request->input();

            $pet = PetsManager::addPet($payload);
            if (!$pet) {
                throw new \Exception('unable to create pet');
            }

            return response([
                'ok' => true,

                'data' => [
                    'pet_id' => $pet->id,
                    'pet' => $pet
                ]
            ])->setStatusCode(201);
        } catch (\Exception $e) {
            return response([
                'ok' => false,

                'error' => $e->getMessage()
            ])->setStatusCode(500);
        }
    }

    public function update(Request $request)
    {
        try {
            throw new \Exception('not implemented');
        } catch (\Exception $e) {
            return response([
                'ok' => false,

                'error' => $e->getMessage()
            ])->setStatusCode(500);
        }
    }

    public function delete(Request $request, string $projectId)
    {
        try {
            throw new \Exception('not implemented');
        } catch (\Exception $e) {
            return response([
                'ok' => false,

                'error' => $e->getMessage()
            ])->setStatusCode(501);
        }
    }
}
```

### 8.1.3.3 Services

Services write and retrieve data from the DB, do validation, etc. etc.

Services MUST go inside `/app/Services`--you should create this directory if it's not there already.

Classes for services MUST be named by pluralizing the model name and appending the word "Manager". For instance: `\App\Services\PetsManager`.

All services MUST be static classes.

Methods for CRUD are:

- `getPets/getPetById` -- for retrieving objects
- `createPet` -- should use a private function `_savePet` for creating + updating, with an `$id` parameter
- `updatePet` -- should use a private function `_savePet` for creating + updating, with an `$id` parameter
- `deletePet`

#### Validation

For validating object, we take advantage of the model's rules:

```php
save_pet($pet_data) {
    // validate
    $rules = \App\Models\Pet::rules();

    $validator = \Illuminate\Support\Facades\Validator::make($pet_data, $rules);

    if ($validator->fails()) {
        throw new \Exception('validation problem(s): ' . $validator->messages());
    }

    // etc...
}
```

## 8.1.3.4 Auth

For authentication and autorization, we are using this technique to figure out who's who, by hardcoding in `.env`: https://stackoverflow.com/a/37095666/2178206.

Then, to restrict according to user type, we add to the `User` model:

```php
class User extends Authenticatable
{
    use Notifiable, HasApiTokens, SoftDeletes;

    protected $dates = ['deleted_at'];
    protected $appends = array('user_type'); // <-- ADD THIS

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        // ...
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password',
        'remember_token',
        'activation_code'
    ];

    public function getUserTypeAttribute() // <-- ADD THIS
    {
        return in_array($this->id, [7]) ? 'admin' : 'nobody';
    }
}
```

One semi-cool technique for `user_type` is to have user types as integers.

Given the following roles:

```php
define('AUTH_USER_TYPE_SYSTEM', 100);
define('AUTH_USER_TYPE_ADMIN', 90);
define('AUTH_USER_TYPE_EDITOR', 80);
define('AUTH_USER_TYPE_SUBSCRIBER', 70);
define('AUTH_USER_TYPE_DEFAULT', 1);

We could implement something like this:
```php
if (Auth::user()->user_type >= AUTH_USER_TYPE_EDITOR) {
    // view unpublished posts
}
```

Might be dumb, though.

## 8.1.3.5 DB seeders

See: https://stackoverflow.com/a/32856878

# 8.2 React

Anything that takes more than 1 hour with jQuery MUST BE implemented with ReactJS.

## 8.2.1 Setup

- start with `create-react-app`
- MUST remove the service worker--if anything because it fucks up viewing updates while developing
- MUST use hot reloading. This works great in 08/2018: https://daveceddia.com/hot-reloading-create-react-app/
- CAN use following if need to customize `build` folder: https://github.com/facebook/create-react-app/issues/1354#issuecomment-428741120 to control build folder (ex., to have `index.php` instead of `index.php`.

## 8.2.2 Generic guidelines

- have 1 single `App` component that handles shared data (auth, caches, anything that gets used on every/most pages of the app and the user CANNOT modify)
- `App` passes props to `Page` components. Each page of the app is a component that handles data, and passes it down to its children
- anything other than `App` and `Page` components are dumb components and CANNOT save or handle any data whatsoever, only `App` and `Page`s can call APIs

## 8.2.3 Animations

For animations, use https://github.com/reactjs/react-transition-group/tree/v1-stable/ with a combination of animate.css and custom animations (if necessary).

This is an example of how to use with `animate.css`:

```jsx
<CSSTransitionGroup
  transitionName={{
    enter: "animated",
    enterActive: "rubberBand",
    leave: "animated",
    leaveActive: "fadeOutRight"
  }}
  transitionAppear={true}
>
  {
    this.state && this.state.show ? (
      <h1>Fading at Initial Mount</h1>
    ) : ''
  }
</CSSTransitionGroup>
```
NOT SURE if we should wrap this stuff, as it's kind of verbose. Something like the following?

```jsx
<Anim enter="rubberBand" leave="fadeOutRight">
  ...
</Anim>
```

No idea.

# 8.2.1 Dashboard app

Simple dashboard/control panel like app.

# 8.2.1.1 Page

There should be a base "page" that can be used for all other pages.

This is a screenshot of all possible elements in the `colibri-libri-dashboard` implementation:

![](https://i.imgur.com/WxkwHva.png)

# 9. Workflow

# Kanban-like workflow

## Collecting requirements from the client

Librì will collect requirements from the client, project manager will provide developers with a detailed explanation of what needs to be done, why it needs to be done, and what needs to happen for it to be considered done. Each of these things is called "Story".

## Milestones

Stories will be turned into GitHub milestones by the project manager. Each milestone may have a deadline if required, and its name will always start with a progressive number (ex. "001 - First milestone").

More info on milestones can be found here: https://guides.github.com/features/issues/.

## Issues

Developers—with the help of the project manager if needed—will create subtasks in the form of GitHub issues of what needs to be done to complete each milestone, and will assign the issue to the milestone.

This will allow to check progress for each milestone.

### Assignment

If applicable, and issue will be assigned to the person responsible for closing it.

### Labels

All repos will have the labels outlined in https://blog.adam-marsden.co.uk/better-github-labels-f1360b43e0a7 (updated script to create from console: https://gist.github.com/nkkollaw/2693f36bc15200abe637bad044a43345). Any additional label will follow that convention.

Issue labels will be used to convey additional information on the issue. The most important aspect is their status:

- Status: On Hold (new issue)
- Status: Feedback Needed (issue is not clear, need more info)
- Status: Confirmed (issue is taken by dev)
- Status: In Progress (dev is working on issue)
- Status: Review Needed (dev is done, needs code review)
- Status: Completed (issue is completed)

If issues are filed by a developer, they can start with the status "Confirmed". In all other cases, "On Hold" must be used.

When a dev starts working on an issue, he must change the label to "In Progress". Devs should not have more than 1 issue in progress at any given time. He can then change the label to "Review Needed" or "Completed" when done.

More info on issues can be found here: https://guides.github.com/features/issues/.

## Branches

Each repo will have at least 2 branches:

- Master - stable code
- Develop - possibly unstable but working code

Devs will create branches to start working on an issue. Once done, they will create a pull request, and ask for code review via the GitHub interface. In the commit, we must reference the issue in the form of: "Fix #12345".

GitHub offer a good workflow for asking for changes and submitted fixes.

When the fixes are submitted, the code can be merged into the "Develop" branch.

## Testing

Testers will test the code on the develop branch, and create issues with the proper label in case of bugs.

## Master

When the code is OK, the branch can be merged into "Master".

## Deployment

Depends on needs and project size
