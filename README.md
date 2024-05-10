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