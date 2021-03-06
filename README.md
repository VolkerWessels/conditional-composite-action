# conditional-composite-action
Conditional composite action as a workaround until possibility to use 'if:' option for action steps of composite actions is available
Use this workaround until this composite action [issue](https://github.com/actions/runner/issues/834) is fixed 

Credits to [ChristopherHX](https://github.com/ChristopherHX/conditional)

Polyfill missing if of actions/runner

Usage:
You have to surround the if condition with ${{.,,}} otherwise it won't work.
The pipe symbol ( | ) after step: is required, because inputs have to be strings.
Expression syntax will even work on places github wouldn't allow (step property)
Pre steps aren't executed, because they won't work with local actions.
- uses: ChristopherHX/conditional@b4a9649204f81002ec9a4ef7d4bf7d6b2ab7fa55
  with:
  if: ${{steps.cacheaction.outputs.cache-hit != 'true'}}
  step: |
  uses: actions/setup-node@v2
  with:
  node-version: ${{inputs.nodever}}
  Full example

How does it work?
This small composite action create a temporary child composite action in ./.github/conditional/tmp/, 
based on the evaluated condition and your provided step, therefore you can skip a uses directive. 
If the condition evaluates to false, it executes an empty action instead. 
The usage of - uses: actions/github-script@v4 allows this action to work on 
linux / windows / mac without the need for bash or downloading any other shell.
