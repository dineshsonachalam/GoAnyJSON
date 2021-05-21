


### Pending work:

1. CRUD functionality.
```
// Postgres table schema:
DROP TABLE IF EXISTS app_user;
DROP TABLE IF EXISTS gist;

CREATE TABLE IF NOT EXISTS app_user(
   id VARCHAR(255) PRIMARY KEY NOT NULL,
   username VARCHAR(255) NOT NULL,
   email_id VARCHAR(255) NOT NULL,
   created_at TIMESTAMPTZ DEFAULT Now() 
);


CREATE TABLE IF NOT EXISTS gist(
   id VARCHAR(255) PRIMARY KEY NOT NULL,
   filename VARCHAR(255) NOT NULL,
   url VARCHAR(255) NOT NULL,
   github_user_id VARCHAR(255) NOT NULL references app_user(id),
   created_at TIMESTAMPTZ DEFAULT Now()
);
--  1. Create a user

-- INSERT INTO app_user(id, username, email_id)
-- VALUES('1','dineshsonachalam', 'dineshsonachalam@gmail.com');


-- 2. Create gist

-- INSERT INTO gist(id, filename, url, github_user_id)
-- VALUES('104', 'example4.json', 'https://gist.github.com/4', '1');

-- 3. Get all gist created by a user

-- SELECT filename, url, date_trunc('second', created_at) as created_at FROM gist 
-- WHERE github_user_id='1' ORDER  BY created_at DESC NULLS LAST;

-- 4.  Delete a gist created by a user

-- DELETE FROM gist
-- WHERE id='102' AND github_user_id='1';
```

**Operations:**
1. Create user.
2. Validate if the user_id is available in the table users.
3. User will be able to create gist after Oauth validation.
4. List down all the gist created by the user by filtering gists table by github_user_id. 
   Sort by last modified data.
5. Delete gists by gist id.

**NOTE**
Keys that I'm interested to look from the Github Oauth response.
```
{
    "id": 12673979, // github user id
    "login": "dineshsonachalam",
    "email": "dineshsonachalam@gmail.com" // github user email
}
```
2. Migrate from HTTP2 to GRPC using grpc gateway.
3. Add Github Oauth to the Backend.
4. JWT
5. Develop frontend.
6. Add test for frontend and backend.
7. k8 deployment.


#### Go guideline
1. To initialize a project with go module, run:
```go mod init your-project-name```

2.Add missing and/or remove unused modules: 
```go mod tidy```

3. You can even vendor the modules in your project directory:
```
go mod vendor
```

Creating gists:
```
curl --location --request POST 'https://api.github.com/gists' \
--header 'Authorization: Bearer <ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--header 'Cookie: _octo=GH1.1.329936248.1614167878; logged_in=no' \
--data-raw '{"public":true,"files":{"test.txt":{"content":"String file contents"}}}'
```

#### Useful resources:
1. K8 postgrest config: https://github.com/IBM/Kubernetes-container-service-GitLab-sample/blob/master/kubernetes/postgres.yaml



### Gist table:
1. Create filename and last modified timestamp column.
2. Create table if not exists.


### Rest API:
1. /gists -> Create a gist (POST)
2. /login -> User login (POST)
3. /gists -> Get all gists of authenticated user. (GET)
4. /gists/{gist_id} -> Delete a gist (DELETE)