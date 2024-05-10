## Controlling Workflow

To Controlling Workflow, we use `if` condition on this workflow, 
EX:-
```
    - name: Test code
        id: run-tests
        run: npm run test
    - name: Upload test report
        if: failure() && steps.run-tests.outcome == 'failure'
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
```

Example shows that if `Test Code` fail that time `Upload test report` exicute and storing fail report

### adding if condition on jobs
```
report:
    needs: [lint, deploy]
    if: failure()
```

### if condition in caching

```
    - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
    - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
```

we can totaly skip if we already have cache


### Continue on error
```
    - name: Test code
      continue-on-error: true
      run: npm run test
```
it means if task fail, it also go further


## Matrix

```
name: Matrix demo
on: push
jobs:
    build:
        continue-on-error: true
        strategy:
            matrix:
                node-version: [12, 14, 16]
                operating-system: [ubuntu-latest, windows-latest]
        runs-on: ${{ matrix.operating-system }}
        steps:
            - name: Get Code
              uses: actions/checkout@v4
            - name: Install Node
              uses: actions/setup-node@v3
              with:
                node-version: ${{ matrix.node-version }}
            - name: Install Dependancy
              run: npm ci
```

we can run test with diff versions so that clear not to fail in any version