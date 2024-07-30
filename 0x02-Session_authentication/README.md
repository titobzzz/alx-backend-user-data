# Session Authentication

This project contains tasks for learning to authenticate a user through session authentication.

## Tasks To Complete

+ [x] 00. **Et moi et moi et moi!**
  + Copy all your work of the [0x06. Basic authentication](../0x01-Basic_authentication/) project in this new folder.
  + In this version, you implemented a **Basic authentication** for giving you access to all User endpoints:
    + `GET /api/v1/users`.
    + `POST /api/v1/users`.
    + `GET /api/v1/users/<user_id>`.
    + `PUT /api/v1/users/<user_id>`.
    + `DELETE /api/v1/users/<user_id>`.
  + Now, you will add a new endpoint: `GET /users/me` to retrieve the authenticated `User` object.
    + Copy folders `models` and `api` from the previous project [0x06. Basic authentication](../0x01-Basic_authentication/).
    + Please make sure all mandatory tasks of this previous project are done at 100% because this project (and the rest of this track) will be based on it.
    + Update `@app.before_request` in [api/v1/app.py](api/v1/app.py):
      + Assign the result of `auth.current_user(request)` to `request.current_user`.
    + Update method for the route `GET /api/v1/users/<user_id>` in [api/v1/views/users.py](api/v1/views/users.py):
      + If `<user_id>` is equal to `me` and `request.current_user` is `None`: `abort(404)`.
      + If `<user_id>` is equal to `me` and `request.current_user` is not `None`: return the authenticated `User` in a JSON response (like a normal case of `GET /api/v1/users/<user_id>` where `<user_id>` is a valid `User` ID)
      + Otherwise, keep the same behavior.
+ [x] 2. **Create a session**
  + Update `SessionAuth` class:
    + Create a class attribute `user_id_by_session_id` initialized by an empty dictionary.
    + Create an instance method `def create_session(self, user_id: str = None) -> str:` that creates a Session ID for a `user_id`:
    + Return `None` if `user_id` is `None`.
    + Return `None` if `user_id` is not a string.
    + Otherwise:
      + Generate a Session ID using `uuid` module and `uuid4()` like `id` in `Base`.
      + Use this Session ID as key of the dictionary `user_id_by_session_id` - the value for this key must be `user_id`.
      + Return the Session ID.
    + The same `user_id` can have multiple Session ID - indeed, the `user_id` is the value in the dictionary `user_id_by_session_id`.
  + Now you an "in-memory" Session ID storing. You will be able to retrieve an `User` id based on a Session ID.

+ [x] 3. **User ID for Session ID**
  + Update `SessionAuth` class:
  + Create an instance method `def user_id_for_session_id(self, session_id: str = None) -> str:` that returns a `User` ID based on a Session ID:
    + Return `None` if `session_id` is `None`.
    + Return `None` if `session_id` is not a string.
    + Return the value (the User ID) for the key `session_id` in the dictionary `user_id_by_session_id`.
    + You must use `.get()` built-in for accessing in a dictionary a value based on key.
  + Now you have 2 methods (`create_session` and `user_id_for_session_id`) for storing and retrieving a link between a User ID and a Session ID.

+ [x] 5. **Before request**
  + Update the `@app.before_request` method in [api/v1/app.py](api/v1/app.py):
    + Add the URL path `/api/v1/auth_session/login/` in the list of excluded paths of the method `require_auth` - this route doesn't exist yet but it should be accessible outside authentication
    + If `auth.authorization_header(request)` and `auth.session_cookie(request)` return `None`, `abort(401)`

+ [x] 6. **Use Session ID for identifying a User**
  + Update `SessionAuth` class:

  + Create an instance method `def current_user(self, request=None):` (overload) that returns a `User` instance based on a cookie value:

    + You must use `self.session_cookie(...)` and `self.user_id_for_session_id(...)` to return the User ID based on the cookie `_my_session_id`.
    + By using this User ID, you will be able to retrieve a `User` instance from the database - you can use `User.get(...)` for retrieving a `User` from the database.
  + Now, you will be able to get a User based on his session ID.

+ 