# Cypress Assignment

For this assignment, you will need:

1. [Metamask Chrome Extension](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en)

You will be writing cypress test against the [following website](https://metamask.github.io/test-dapp/):

All the features in the website are testable. For this assignment, we only need a couple of them:

## Test the default state of the website when metamask is not connected.

[Not Connected](./not%20connected.png)

## Test the state of the website when metamask is connected

[Connected](./connected.png)

## Test encryption and decryption of sample text

[Encryption and Decryption](./encryption-decreption.png)

There are library available to test metamask with cypress. You can use any of them including:

1. [Synpress](https://github.com/Synthetixio/synpress)
2. [Cypress Metamask V2](https://www.npmjs.com/package/cypress-metamask-v2)

# Complete unit tests for the following function:

```
const GENERIC_ERROR = "Something went wrong. Please try again later.";

export function parseValidationErrorMessage(error: any): string[] {
  if (!error) {
    return [GENERIC_ERROR];
  }
  const { message, errors } = error;
  if (!errors) {
    if (message && typeof message === "string") {
      return [message];
    }
    return [GENERIC_ERROR];
  }
  const errorResultArray: string[] = [];
  getErrorMessages(errors, errorResultArray, "");
  return errorResultArray;
}

function getErrorMessages(error: any, arr: string[], parentKey: string): void {
  if (typeof error === "string") {
    const msg = parentKey ? `${parentKey} ${error}` : error;
    arr.push(msg);
    return;
  }
  if (Array.isArray(error)) {
    for (const val of error) {
      getErrorMessages(val, arr, parentKey);
    }
    return;
  }
  if (typeof error === "object") {
    for (const [key, value] of Object.entries(error)) {
      getErrorMessages(value, arr, parentKey ? `${parentKey}.${key}` : key);
    }
    return;
  }
}
```

## A first test is provided as a sample.

The test is written in jest. Feel free to use any other library of your choice for unit tests.

```import { parseValidationErrorMessage } from "utils/parseValidationErrorMessage";

test("validate 422 errors from contract creation post api", () => {
  const input = {
    message: "Validation error",
    errors: {
      contractTitle: ["ensure this value has at least 1 characters"],
      contractorJobTitle: ["field required"],
      scopeOfWork: ["ensure this value has at least 1 characters"],
      contractorFirstName: ["ensure this value has at least 1 characters"],
      contractorLastName: ["ensure this value has at least 1 characters"],
      contractorEmail: ["value is not a valid email address"],
      contractorAddress: {
        country: {
          name: ["field required"],
          iso2: ["field required"],
        },
        state: {
          name: ["field required"],
          iso2: ["field required"],
        },
      },
    },
  };
  const expected = [
    "contractTitle ensure this value has at least 1 characters",
    "contractorJobTitle field required",
    "scopeOfWork ensure this value has at least 1 characters",
    "contractorFirstName ensure this value has at least 1 characters",
    "contractorLastName ensure this value has at least 1 characters",
    "contractorEmail value is not a valid email address",
    "contractorAddress.country.name field required",
    "contractorAddress.country.iso2 field required",
    "contractorAddress.state.name field required",
    "contractorAddress.state.iso2 field required",
  ];
  const actual = parseValidationErrorMessage(input);
  expect(actual).toEqual(expected);
});
```

We would like you to extend the testcase with multiple data considering the general issues like undefined, null, etc that happens in javascript development.
