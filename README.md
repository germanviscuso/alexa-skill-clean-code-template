# alexa-skill-clean-code-template

<https://github.com/javichur/alexa-skill-clean-code-template>

## Setup

- ASK CLI (`npm install -g ask-cli`)
- Visual Code (<https://code.visualstudio.com/)>
- Alexa Skill Kit (ASK) Toolkit for vscode (<https://marketplace.visualstudio.com/items?itemName=ask-toolkit.alexa-skills-kit-toolkit>)

## How to create a new Alexa Skill using this template

1. Configure ASK CLI with AWS profile (credentials):

```shell
ask init
```

You must edit ~/.aws/credentials file to add a valid AWS profile (aws_access_key_id, aws_secret_access_key). More info: <https://docs.aws.amazon.com/es_es/cli/latest/userguide/cli-chap-configure.html>

2. Create a new skill from this template (<https://github.com/javichur/alexa-skill-clean-code-template>):

```shell
ask new --url https://github.com/javichur/alexa-skill-clean-code-template.git
? Please type in your new skill name, alphanumeric only: my-new-skill
```

3. Install dependencies:

```shell
cd my-new-skill/lambda/custom
npm install
```

## How to take advantage of this template

1. Control your Code Style using Airbnb eslint rules:

  ```shell
  npm run eslint
  ```

2. Control the quality of your code using SonarQube (I assume Sonar running on localhost:9000):

```shell
npm run sonar
start http://localhost:9000/dashboard?id=my-new-skill
```

3. Add text strings for all languages in `strings` folder.

4. Use vscode and press F5 to start Unit Tests and **debug with breakpoints in your code**.

5. Deploy the skill easily with cli:

```shell
ask deploy
```

## How this template was created from Amazon-provided "Hello world" template

1. Configure ASK CLI with AWS profile (credentials):

```shell
ask init
```

You must edit ~/.aws/credentials file to add a valid AWS profile (aws_access_key_id, aws_secret_access_key, region). More info: <https://docs.aws.amazon.com/es_es/cli/latest/userguide/cli-chap-configure.html>

2. Create a new skill from `Hello World` template (<https://github.com/alexa/skill-sample-nodejs-hello-world>):

```shell
ask new
? Please select the runtime Node.js V8
? List of templates you can choose Hello World
? Please type in your skill name:  alexa-skill-clean-code-template
```

The `.ask/config` file will be contain:

```json
{
  "deploy_settings": {
    "default": {
      "skill_id": "",
      "was_cloned": false,
      "merge": {}
    }
  }
}
```

3. Edit `skill.json` to deploy Lambda function in Europe:

```json
"apis": {
  "custom": {
    "endpoint": {
      "sourceDir": "lambda/custom",
      "uri": "ask-custom-clean-code-skill-template-default"
    },
    "regions": {
      "EU": {
        "endpoint": {
          "sourceDir": "lambda/custom",
          "uri": "ask-custom-clean-code-skill-template-default"
        }
      }
    }
  }
}
```

4. Deploy

```shell
ask deploy
```

Or deploy only Lambda function

```shell
ask deploy lambda
```

The `ask deploy` command invokes hooks in order to install dependencies.

5. Test interaction model using `Utterance Profiler` (<https://developer.amazon.com/alexa/console/ask>)

6. Test your skill using **interactive** `ask dialog` (beta)

```shell
ask dialog --locale "en-US"
User  >  start hello world
Alexa >  Welcome to the Alexa Skills Kit, you can say hello!
User  >  help
Alexa >  You can say hello to me!
User  >  hello
Alexa >  Hello World!
```

7. Test your skill using simulator (<https://developer.amazon.com/alexa/console/ask>)

8. Test JSON request/response using `ask simulate`

```shell
ask simulate --locale "en-US" --text "start hello world"
```

9. Automating Unit Tests using Bespoken Tools.

```shell
npm install bespoken-tools --save-dev
```

10. Clean code. ESLINT (with airbnb rules) + Sonar added. See package.json

```shell
npm run eslint
npm run sonar-eslint
```

11. Location. `addRequestInterceptors()` string files added and strings replaced.

12. Refactor handlers.

13. `IntentReflectorHandler` added.

14. Add APL support in Skill Manifest (<https://developer.amazon.com/es/docs/alexa-presentation-language/apl-select-the-viewport-profiles-your-skill-supports.html>).

```json
"interfaces": [
  {
    "type": "ALEXA_PRESENTATION_APL",
    "supportedViewports": [
      {
        "mode": "HUB",
        "shape": "ROUND",
        "minWidth": 480,
        "maxWidth": 480,
        "minHeight": 480,
        "maxHeight": 480
      },
      {
        "mode": "HUB",
        "shape": "RECTANGLE",
        "minWidth": 1024,
        "maxWidth": 1024,
        "minHeight": 600,
        "maxHeight": 600
      },
      {
        "mode": "HUB",
        "shape": "RECTANGLE",
        "minWidth": 1280,
        "maxWidth": 1280,
        "minHeight": 800,
        "maxHeight": 800
      },
      {
        "mode": "TV",
        "shape": "RECTANGLE",
        "minWidth": 960,
        "maxWidth": 960,
        "minHeight": 540,
        "maxHeight": 540
      },
      {
        "mode": "HUB",
        "shape": "RECTANGLE",
        "minWidth": 960,
        "maxWidth": 960,
        "minHeight": 480,
        "maxHeight": 480
      }
    ]
  }
],
```

15. Add APL templates `/apl`.
// TODO

16. Use APL with components.
// TODO

17. Access to DynamoDB

```shell
npm install dynamola
```

Use it:

```javascript
const Dynamola = require('dynamola');
let myDb = new Dynamola("nombre-tabla-en-dynamodb", "nombre-primary-key-en-dynamodb", null);

myDb.getItem(userID).then((data) => {
  if(!data){
      // item no existe
  }
  else {
    // item devuelto OK
  }
})
.catch((err) => {
  // error al acceder a dynamodb
});
```

// TODO

18. Acess to external APIs
// TODO

19. Deploy using Alias and Lambda versions.
// TODO

## Known problems

1. Error invoking `ask deploy`: "No se puede cargar el archivo `\hooks\pre_deploy_hook.ps1` porque la ejecución de scripts está deshabilitada en este sistema". Solution:

```shell
Set-ExecutionPolicy -Scope CurrentUser
$> unrestricted
```

**IMPORTANT**: If you create a new skill project from a skill template that contains hook scripts, ASK CLI will run them. You should only use skill templates from sources that you trust.
