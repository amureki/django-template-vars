# Django Template Vars

The goal of this project is to provide a way to pass variables to Django templates,
in a single container, and to be able to access them in a simple way.

A container ensures that you always know what variables are generally available in the template
to differentiate them from the variables passed to the context in the view.

## Example

Define in the `context_processors.py` file the variables that will be available in the template context.

```python
from django.conf import settings

def get_window_variables(self):
    """A dictionary of variables that will be available in the window object."""
    return {
        "user": {
            "id": self.request.user.id,
        },
        "support_email": "support@example.com",
    }

def get_global_variables(self):
    """A dictionary of variables that will be available in the template context."""
    return {
        "DEBUG": settings.DEBUG,
        "user": {
            "id": self.request.user.id,
        }
    }
```

Then, in the `base.html` file, you can access the variables like this:

```html
# base.html

<!-- access directly in the template -->
{{ template_vars.user.id }}
{{ template_vars.DEBUG }}

<!-- access in the window object -->

<script>
    console.log(window.appVars)
    // > {
    // >   user: { id: 1 },
    // >   support_email: "support@example.com"
    // > }
    console.log(window.appVars.user.id)
    // > 1
</script>
```