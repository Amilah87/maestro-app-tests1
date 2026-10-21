# WebdriverIO Native Demo App: Maestro tests

Automated Android tests for the **sign-up**, **login** and **forms** screens of the
[WebdriverIO Native Demo App v2.2.0](https://github.com/webdriverio/native-demo-app/releases/tag/v2.2.0),
written with [Maestro](https://maestro.mobile.dev).

## Setup and run

1. Install Maestro: `curl -fsSL "https://get.maestro.mobile.dev" | bash`
2. Start an Android emulator (or connect a device with USB debugging on).
3. Download `android.wdio.native.app.v2.2.0.apk` from the release page into `apps/`.
4. Install it: `adb install apps/android.wdio.native.app.v2.2.0.apk`
5. Run all tests: `maestro test .`
   - One flow: `maestro test flows/login_success.yaml`


## Why Maestro

- Flows are short, declarative YAML, so a reviewer can read a test in seconds.
- It waits for elements automatically (no `sleep`, no explicit waits) which removes the
  most common source of flaky mobile tests.
- Setup is one binary plus an emulator, so it is quick to run from a fresh clone.
- Trade-off: less programmatic flexibility than Appium (no loops over data or
  custom assertions in code). For three simple screens that is a good trade.

## Project layout

```
config.yaml        # tells Maestro which flows to run (flows/ only)
flows/             # one file per scenario, each independent
helpers/           # reusable sub-flows (open login / sign-up / forms screens)
```

Every flow starts with `launchApp: clearState: true`, so tests never depend on each other
or on run order.

## Scenarios covered

| Area | Scenario | Asserts |
|------|----------|---------|
| Sign up | Valid details | "Signed Up!" dialog and message |
| Sign up | Invalid email | email validation message, no dialog |
| Sign up | Mismatched passwords | mismatch message, no dialog |
| Login | Valid credentials | "Success" dialog and message |
| Login | Invalid email | email validation message, no dialog |
| Login | Password under 8 chars | length validation message, no dialog |
| Forms | Typed text | text is echoed back |
| Forms | Switch | label flips OFF/ON on each tap |
| Forms | "Active" button | native alert text, dismissed with OK |

## What I would do next

- Run in CI (GitHub Actions with an Android emulator) and publish JUnit results.
- Add negative cases for empty fields and the "Inactive" button.
- Add the optional iOS simulator build.
- Move shared test data (emails, passwords) into `env` variables.

## Bugs / observations
- Bottom tab labels are very small and thin on a black background; a larger
  font or heavier weight would improve readability.
- The selected tab (Login) is shown only by colour (orange vs white), and the
  orange label looks dimmer than the white ones. An indicator such as an
  underline or bold text would make the active tab clearer.
- Tab icons mix outlined and solid styles, so the row looks uneven.
- Tab bar spacing above the system navigation bar is tight.
- the error message should disappear once the field is cleared or corrected

## AI usage
- I used Claude to draft the initial plan and README. Manully scrpting and ran every flow on an emulator (Maestro Studio) and check the selectors correctness and expected texts.
