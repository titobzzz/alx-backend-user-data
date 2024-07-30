# Basic authentication

This project contains tasks for learning to authenticate a user using the Basic authentication scheme.


+ [x] 1. **Error handler: Forbidden**
  + Edit [api/v1/app.py](api/v1/app.py):
    + Add a new error handler for this status code, the response must be:
      + A JSON: `{"error": "Forbidden"}`.
      + Status code `403`.
      + You must use `jsonify` from Flask.
  + For testing this new error handler, add a new endpoint in [api/v1/views/index.py](api/v1/views/index.py):
    + Route: `GET /api/v1/forbidden`.
    + This endpoint must raise a 403 error by using `abort` - [Custom Error Pages](https://flask.palletsprojects.com/en/1.1.x/patterns/errorpages/).
  + By calling `abort(403)`, the error handler for 403 will be executed.

+ [x] 2. **Auth class**
  + Create a class to manage the API authentication.
    + Create a folder [api/v1/auth](api/v1/auth).
    + Create an empty file [api/v1/auth/__init__.py](api/v1/auth/__init__.py).
    + Create the class `Auth`:
      + In the file [api/v1/auth/auth.py](api/v1/auth/auth.py).
      + Import `request` from flask.
      + Class name `Auth`.
      + Public method `def require_auth(self, path: str, excluded_paths: List[str]) -> bool:` that returns `False` - `path` and `excluded_paths` will be used later, now, you don't need to take care of them.
      + Public method `def authorization_header(self, request=None) -> str:` that returns `None` - `request` will be the Flask request object.
      + Public method `def current_user(self, request=None) -> TypeVar('User'):` that returns `None` - `request` will be the Flask request object.
    + This class is the template for all authentication system you will implement.
