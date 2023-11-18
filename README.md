# ed-laravel-guards-gates-policies
By [article](https://sagardhiman021.medium.com/laravel-guards-gates-and-policies-understanding-implementation-and-key-differences-5d56186603c7)

## 1 Guards

Guards in Laravel define how users are authenticated in your application. They determine where the application should look for user credentials and how to verify them. Laravel provides multiple guard drivers out of the box, such as “web” (for browser sessions) and “api” (for API token authentication).

### How to Implement Guards:

You can configure guards in the config/auth.php file. For example, the "web" guard might be configured to use session-based authentication, while the "api" guard could use token-based authentication.

Here’s a simplified example of a “web” guard configuration:

```
'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],
],
```

## 2 Gates

What are Gates?

Gates are used for defining and checking authorization policies in Laravel. They allow you to define fine-grained access control rules for different actions or operations in your application. Gates are often used within your code to check if a user is authorized to perform a specific action.

### How to Implement Gates:

You define gates in the AuthServiceProvider class using the Gate facade. You can define a gate by specifying a name and a closure that defines the authorization logic.

Here’s a simplified example of defining a gate that checks if a user can update a blog post:

```
use Illuminate\Support\Facades\Gate;

public function boot()
{
    $this->registerPolicies();

    Gate::define('update-post', function ($user, $post) {
        return $user->id === $post->user_id;
    });
}
```

## 3 Policies

What are Policies?

Policies are similar to gates but provide a more organized way to manage authorization logic for specific models in your application. Each model can have its own policy class that defines the authorization rules for that model’s actions (e.g., creating, updating, deleting).

### How to Implement Policies:

To create a policy for a model, you can use the artisan command:

```
php artisan make:policy PostPolicy
```

This will generate a PostPolicy class. You can define the authorization rules for the Post model's actions in this class. Policies are typically used with the authorize method in your controller.

Here’s a simplified example of a PostPolicy class that checks if a user can update a post:

```
class PostPolicy
{
    public function update(User $user, Post $post)
    {
        return $user->id === $post->user_id;
    }
}
```

Now, in your controller, you can use the authorize method to check if a user is authorized to update a post:

```
public function update(Post $post)
{
    $this->authorize('update', $post);

    // Update the post here
}
```

## Key Differences:
- Guards: Guards handle user authentication and define where the application should look for user credentials (e.g., sessions, tokens).
- Gates: Gates are used to define and check authorization policies for specific actions or operations in your application.
- Policies: Policies are used to define authorization policies for specific models, making it easier to manage rules for CRUD operations related to those models.
