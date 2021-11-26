# Node Server

## nvm
This project was built alongside NVM from `https://github.com/nvm-sh/nvm` to ensure the underlying node version was consistently updated.

## .nvmrc
`.nvmrc` contains `lts/Erbium`, which at the time of writing resolves to v12.22.3. This project should be maintained to conform to lts/Erbium instead of static versions of node.

To automatically load the content of `.nvmrc` the following should be added to ~/.bash_profile
```
	load_nvmrc() {
	if [[ -f .nvmrc && -r .nvmrc ]]; then
		nvm use
	elif [[ $(nvm version) != $(nvm version default)  ]]; then
		echo "Reverting to nvm default version"
		nvm use default
	fi
	}
	load_nvmrc;
```

## Development server

Run `npm run dev` for a dev server. Navigate to `http://local.api.server/`. The application will automatically refresh when file changes are made.

You will also need an entry in `/etc/hosts` with the following
```
127.0.0.1	local.api.server
```
This is necessesary because chrome will not send CORS to the external applications which this uses without running unsafe networking. However running unsafe networking will shield the developer from any network issues an end user would face.
# Code
## Scaffolding and Style
The following approach should be followed for new code, and used to clean the application up, refactoring other code where sensible to do so.

### Interacting with a Model/Entity

```
/src/routes/refactor/{tableName}.route.ts
/src/db/models/{tableName}.model.ts -> Should always be timestamp+paranoid
/src/controller/refactor/{tablename}.controller.ts -> Extends CrudController
/src/service/refactor/{tablename}.service.ts -> Extends CrudService
```

example:

```
/src/routes/refactor/user.role.route.ts -> Must be added to /src/routes/index.ts
/src/db/models/userRole.model.ts
/src/controller/refactor/user.role.controller.ts
/src/service/refactor/user.role.service.ts
```

### Recommended Way to Navigate the Code
Look for files by their logical name for existing functionality rather than manually navigating the code. If in doubt consult `/src/routes/index.ts`

## Deploy
Any commit to `origin/master` will be automatically deploy to `https://dev-api.server/`

Any commit to `orgin/production` will automatically deploy to `https://api.server/`

Direct commits to `origin/production` are not possble. The release should include a PR from `master` to `production`
