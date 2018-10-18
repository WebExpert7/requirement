# Job_IUPW-W025: Summary

## Purpose

The purpose of this job is to develop an Interbit covenant such that:

1. The covenant object fully implements the interface described in the
   [specification](SPEC.md).

2. The correctness of the covenant's features is demonstrated by
   providing all of the required tests within the
   [specification](SPEC.md).

3. The code you write conforms to the coding style guidelines as defined
   in the `.eslintrc` file, specifically, `prettier-standard`. Run `npm
   run format` to check whether your code is conformant.


## Test Harness

You **must** use the Interbit Test Harness to develop your covenant and
tests:

```bash
npm i --save interbit-test-harness
```

The [Test Harness repo](https://github.com/interbit/test-harness) is
publicly available, for code browsing and
[documentation](https://github.com/interbit/test-harness/tree/master/docs)
is included.


## File locations

- Covenant code should be placed in the `src/` directory.

- Test code and fixtures should be placed in the `test/` directory,
  beside the harness.

Example files are provided in both locations. Make sure that you change
the package name in `package.json` before you start.


## Guidelines

### How to use the test harness

The [test
harness](https://github.com/interbit/test-harness/blob/master/docs/harness.md)
documentation contains information on:

- How to install the test harness.
- How to interact with test harness.
- How the test harness works.
- The example covenant tested by the harness.


### How to read the covenant specification

Before reading the [specification](SPEC.md), see the
[covenant](https://github.com/interbit/test-harness/blob/master/docs/covenant.md)
overview and make sure that you understand the overall expected
structure of the end product.

The responsibilities of a covenant are broken down into actions and
functions to be implemented in each of the four structural elements of
the covenant:

- Reducer Actions
- Saga Actions
- Context Singleton Methods
- Async Worker Methods

The names for each action/method are self explanatory. Method names that
are prefixed with `_` indicate that they are internal to the specific
structural element. These are simply a recommendation in order to ensure
correctness of the operations.

For each of the interface actions within a covenant, the
[specification](SPEC.md) provides a sequence diagram to demonstrate
interactions between all four structural elements.

The "Action Signatures and Tests" section within the
[specification](SPEC.md), describes the actions and behavior that the
covenant is expected to expose to the outside world; i.e. its interface.

Actions initiated by other pieces of the system **always** arrive at the
covenant's reducer. These actions are called `requests`. A `request`
triggers a series of operations and state changes within the covenant. A
covenant eventually has to provide a response to the originator of the
`request`.

For each `request` to be handled by the reducer, a dedicated section
exists within the "Action Signatures and Tests" (within the [specification
document](SPEC.md)) which explains:

- `RequestPayload`: Key/values that are expected within the payload of
  the `request` action.

- `ResponsePayload`: Key/values that are expected within the payload of
  the `response` action.

- `ErrorPayload`: When a `request` cause an error inside the covenant,
  the `response` is expected to have an `error` key with one of the
  explained error `messages`.

- Tests: Human readable explanation of conditions to be tested to ensure
  correctness of workflow.


### Glossary

Terminologies used within our documentation are briefly described in our
[glossary
document](https://github.com/interbit/test-harness/blob/master/docs/glossary.md)


## Support

If you need input or assistance while undertaking this job, email us at:
[devsupport@blockchaintechltd.com](mailto:devsupport@blockchaintechltd.com)
