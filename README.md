## Cypress 

### Quality Assurance
**What is Quality Assurance(QA)** = Quality assurance is a way of preventing mistakes and defects in manufactured products and avoiding problems when delivering products or services to customers

### Types of Quality Testing
1. **Manual Testing** = is performed by a human sitting in front of a computer carefully executing the test steps.

2. **Automation Testing** = means using an `automation tool` to execute your test case suite example of this is using `automation softwares`.

### Unit vs Integration vs End to End

**Unit testing** = testing an isolated part of your app, usually done in combination with shallow rendering. example: a component renders with the default props.

**Integration testing** testing if different parts work or integrate with each other. Usually done with mounting or rendering a component. example: test if a child component can update context state in a parent.

**End to End testing** = a technique used to test whether the flow of an application right `from start to finish is behaving as expected`, Tests are done in a `simulated browser`.


# End to End testing with Cypress 
1. Why is test automation important in software development = can increase the depth and scope of tests to help improve software quality, test automation `can easily execute thousands of different complex test cases` during every test run providing coverage that is impossible with manual tests

2. What is selenium = Selenium is a library but requires a `unit testing framework` or a runner plus an assertions library to build out its capabilities.

e.g : WebDriver.io,Istanbul,Karma ,Robot Framework ,Nightwatch.JS ,Jest ,Jasmine ,Cucumber ,Lettuce ,Protractor ,Mocha ,Tape 

3. What is Cypress = is a JavaScript-based end-to-end testing framework that `doesn't use selenium` at all. 

4. Cypress vs Selenium = Selenium requires testing framework while Cypress can run without a testing framework.


### Cypress Installation 

1. Initilize default `package.json`.

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Pictures/cypress-demo$ npm init -y
Wrote to /home/dev-mentor/Pictures/cypress-demo/package.json:

{
  "name": "cypress-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

2. Install `cypress`

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Pictures/cypress-demo$ npm install cypress --save-dev

> cypress@3.4.1 postinstall /home/dev-mentor/Pictures/cypress-demo/node_modules/cypress
> node index.js --exec install

Installing Cypress (version: 3.4.1)

 ✔  Downloaded Cypress
 ✔  Unzipped Cypress
 ✔  Finished Installation /home/dev-mentor/.cache/Cypress/3.4.1

You can now open Cypress by running: node_modules/.bin/cypress open

https://on.cypress.io/installing-cypress

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN cypress-demo@1.0.0 No description
npm WARN cypress-demo@1.0.0 No repository field.

+ cypress@3.4.1
added 188 packages from 125 contributors and audited 314 packages in 89.972s
found 0 vulnerabilities

```

You should now have the following:

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Pictures/cypress-demo$ ls
cypress  cypress.json  node_modules  package.json  package-lock.json
```

Installing Cypress will take around 2 to 3 minutes, based on your network speed.

3.  Once you have done with the installation part, you will open Cypress for the first time by executing this command at the location where you have your `package.json`

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Pictures/cypress-demo$ ./node_modules/.bin/cypress open
```

![Cypress First run](screenshot-demo/first-running-cypress.gif)


During first time with cypress there are some possible errors you may encounter like missing library or dependency. example is `libgconf-2.so.4: cannot open shared object file: No such file or directory` that you can resolved by `sudo apt -y install libgconf2-4`

Cypress GUI 

![Cypress Window](screenshot-demo/cypress-window.png)

Cypress comes with its own folder structure. This folder is automatically generated when you open Cypress for the first time at that location. It comes with ready-made recipes that show you how to test common scenarios in Cypress.

We keep our test data in a `.json` format inside `fixtures` folder and write tests inside the `integration` folder following the same naming convention. Any custom command will come up under the support folder.

Summary :

1. **cypress-demo/cypress/Fixtures** = Represents the static test data and store as `.json` format. by default cypress generates default fixture which is the `example.json`.

example.json

```
{
  "name": "Using fixtures to represent data",
  "email": "hello@cypress.io",
  "body": "Fixtures are a great way to mock data for responses to routes"
}
```

2. **cypress-demo/cypress/Integration** = The actual test suites contains `spec.js` files. by default cypress generates `examples` default folder.

examples/

```
└── examples
    ├── actions.spec.js
    ├── aliasing.spec.js
    ├── assertions.spec.js
    ├── connectors.spec.js
    ├── cookies.spec.js
    ├── cypress_api.spec.js
    ├── files.spec.js
    ├── local_storage.spec.js
    ├── location.spec.js
    ├── misc.spec.js
    ├── navigation.spec.js
    ├── network_requests.spec.js
    ├── querying.spec.js
    ├── spies_stubs_clocks.spec.js
    ├── traversal.spec.js
    ├── utilities.spec.js
    ├── viewport.spec.js
    ├── waiting.spec.js
    └── window.spec.js
```


### Running our first test suite

**The test case**

Test case name: My first test suite.
- Open web browser and access https://google.com     = Pass or Failed
- At search bar type `boomsourcing`                  = Pass or Failed
- At footer click `About Us`                         = Pass or Failed
- Should redirect to `About Us` page                 = Pass or Failed

**Steps**

1. We need to disable the CORS issue by setting `chromeWebSecurity` into `false` under `cypress-demo/cypress/cypress.json`.

```
{
    "chromeWebSecurity": false
}
```

2. Create `boomsourcing.spec.js` file under `cypress-demo/cypress/integration/` and paste the ff code.

```
//This is where your test suite starts
describe('My first test suite', function () {

    //This function will execute before each test (i.e it())
    beforeEach(function () {

        //Visit google
        cy.visit('https://www.google.com/')

    })

    //The actual suite
    it('Visits the boomsourcing site', function () {

           // Get an input, with name q type 'boomsourcing' and verify that the value is the same as 'boomsourcing'.
           cy.get('input[name=q]')
           .type('boomsourcing')
           .should('have.value', 'boomsourcing')
           
           //Get an input, with name btnK and the text is 'Google Search' then click.
           cy.get('input[name=btnK]').contains('Google Search').click()

           //Click first a tag with link 'https://boomsourcing.com/' and click
           cy.get('a[href="https://boomsourcing.com/"]').first().click()

           //Get span, that has text 'Google Search' then click.
           cy.get('span').contains('About Us').click()

    })
})
```

2. Open cypress GUI 

```
dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Pictures/cypress-demo$ ./node_modules/.bin/cypress open
```

![Click boomspecs](screenshot-demo/running-cypress.gif)

3. Find and click `boomsourcing.spec.js` it will open a new browser that will run our first test suite.

![Click boomspecs](screenshot-demo/boomsourcing.spec.js.png)


Demonstration :

![Click boomspecs](screenshot-demo/boomsourcing.gif)


### API reference 

https://docs.cypress.io/api/api/table-of-contents.html



