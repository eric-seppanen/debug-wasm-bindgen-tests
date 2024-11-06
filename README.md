# Test project for `wasm-pack test` issue [4252](https://github.com/rustwasm/wasm-bindgen/issues/4252).

### Demonstration: It works with older versions of wasm-bindgen.

This project has an empty `lib.rs`, and contains a test file `tests/web.rs` that only contains:
```rust
use wasm_bindgen_test::{wasm_bindgen_test, wasm_bindgen_test_configure};

wasm_bindgen_test_configure!(run_in_browser);

#[wasm_bindgen_test]
async fn empty_test() {
    println!("yay");
}
```

Tests succeed when this project uses `wasm-bindgen` 0.2.92:

```plaintext
$ wasm-pack --version
wasm-pack 0.13.1

$ wasm-pack test --headless --firefox
[INFO]: 🎯  Checking for the Wasm target...
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
   Compiling wasm-bindgen-shared v0.2.92
   Compiling once_cell v1.20.2
   Compiling bumpalo v3.16.0
   Compiling log v0.4.22
   Compiling wasm-bindgen v0.2.92
   Compiling cfg-if v1.0.0
   Compiling scoped-tls v1.0.1
   Compiling dummy-test v0.1.0 (.../debug-wasm-bindgen-tests)
   Compiling quote v1.0.37
   Compiling syn v2.0.87
   Compiling wasm-bindgen-backend v0.2.92
   Compiling wasm-bindgen-macro-support v0.2.92
   Compiling wasm-bindgen-test-macro v0.3.42
   Compiling wasm-bindgen-macro v0.2.92
   Compiling js-sys v0.3.69
   Compiling console_error_panic_hook v0.1.7
   Compiling wasm-bindgen-futures v0.4.42
   Compiling wasm-bindgen-test v0.3.42
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 4.83s
[INFO]: ⬇️  Installing wasm-bindgen...
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running unittests src/lib.rs (target/wasm32-unknown-unknown/debug/deps/dummy_test-9771a554381dc469.wasm)
no tests to run!
     Running tests/web.rs (target/wasm32-unknown-unknown/debug/deps/web-61c35450dab3534c.wasm)
Set timeout to 20 seconds...
Running headless tests in Firefox on `http://127.0.0.1:40171/`
Try find `webdriver.json` for configure browser's capabilities:
Not found
running 1 test

test web::empty_test ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 filtered out
```

### Demonstration: It fails with wasm-bindgen 0.2.93 and above.

```plaintext
$ cargo update -p wasm-bindgen --precise 0.2.93
    Updating crates.io index
    Updating wasm-bindgen v0.2.92 -> v0.2.93
    Updating wasm-bindgen-backend v0.2.92 -> v0.2.93 (latest: v0.2.95)
    Updating wasm-bindgen-macro v0.2.92 -> v0.2.93 (latest: v0.2.95)
    Updating wasm-bindgen-macro-support v0.2.92 -> v0.2.93 (latest: v0.2.95)
    Updating wasm-bindgen-shared v0.2.92 -> v0.2.93 (latest: v0.2.95)
note: pass `--verbose` to see 5 unchanged dependencies behind latest

$ cargo clean
$ wasm-pack test --headless --firefox
[INFO]: 🎯  Checking for the Wasm target...
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
   Compiling wasm-bindgen-shared v0.2.93
   Compiling once_cell v1.20.2
   Compiling bumpalo v3.16.0
   Compiling log v0.4.22
   Compiling wasm-bindgen v0.2.93
   Compiling cfg-if v1.0.0
   Compiling scoped-tls v1.0.1
   Compiling dummy-test v0.1.0 (/home/eric/work/debug-wasm-bindgen-tests)
   Compiling quote v1.0.37
   Compiling syn v2.0.87
   Compiling wasm-bindgen-backend v0.2.93
   Compiling wasm-bindgen-test-macro v0.3.42
   Compiling wasm-bindgen-macro-support v0.2.93
   Compiling wasm-bindgen-macro v0.2.93
   Compiling js-sys v0.3.69
   Compiling console_error_panic_hook v0.1.7
   Compiling wasm-bindgen-futures v0.4.42
   Compiling wasm-bindgen-test v0.3.42
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 5.67s
[INFO]: ⬇️  Installing wasm-bindgen...
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.01s
     Running unittests src/lib.rs (target/wasm32-unknown-unknown/debug/deps/dummy_test-5db8d1540a1f48dc.wasm)
no tests to run!
     Running tests/web.rs (target/wasm32-unknown-unknown/debug/deps/web-4b78f4eba41bde34.wasm)
Set timeout to 20 seconds...
Running headless tests in Firefox on `http://127.0.0.1:43887/`
Try find `webdriver.json` for configure browser's capabilities:
Not found
Failed to detect test as having been run. It might have timed out.
output div contained:
    Loading scripts...

driver status: signal: 9 (SIGKILL)
driver stdout:
    1730926362580	geckodriver	INFO	Listening on 127.0.0.1:43887
    1730926362694	mozrunner::runner	INFO	Running command: MOZ_CRASHREPORTER="1" MOZ_CRASHREPORTER_NO_REPORT="1" MOZ_CRASHREPORTER_SHUTDOWN="1" "/snap/firefox/current/firefox.launcher" "--marionette" "-headless" "-no-remote" "-profile" "/tmp/rust_mozprofilem4F9UU"
    console.warn: services.settings: Ignoring preference override of remote settings server
    console.warn: services.settings: Allow by setting MOZ_REMOTE_SETTINGS_DEVTOOLS=1 in the environment
    1730926362946	Marionette	INFO	Marionette enabled
    1730926363009	Marionette	INFO	Listening on port 42243
    Read port: 42243
    [GFX1-]: RenderCompositorSWGL failed mapping default framebuffer, no dt
    console.error: ({})
    1730926383990	Marionette	INFO	Stopped listening on port 42243

driver stderr:
    *** You are running in headless mode.
    Gtk-Message: 12:52:42.717: Not loading module "atk-bridge": The functionality is provided by GTK natively. Please try to not load it.
    JavaScript error: http://127.0.0.1:37419/run.js, line 9: SyntaxError: The requested module 'http://127.0.0.1:37419/wasm-bindgen-test' doesn't provide an export named: '__wbgtest_cov_dump'

Error: some tests failed
error: test failed, to rerun pass `--test web`

Caused by:
  process didn't exit successfully: `/home/eric/.cache/.wasm-pack/wasm-bindgen-b608dc45898c1197/wasm-bindgen-test-runner /home/eric/work/debug-wasm-bindgen-tests/target/wasm32-unknown-unknown/debug/deps/web-4b78f4eba41bde34.wasm` (exit status: 1)
note: test exited abnormally; to see the full output pass --nocapture to the harness.
Error: Running Wasm tests with wasm-bindgen-test failed
Caused by: Running Wasm tests with wasm-bindgen-test failed
Caused by: failed to execute `cargo test`: exited with exit status: 1
  full command: cd "/home/eric/work/debug-wasm-bindgen-tests" && CARGO_TARGET_WASM32_UNKNOWN_UNKNOWN_RUNNER="/home/eric/.cache/.wasm-pack/wasm-bindgen-b608dc45898c1197/wasm-bindgen-test-runner" GECKODRIVER="/snap/bin/geckodriver" WASM_BINDGEN_TEST_ONLY_WEB="1" "cargo" "test" "--target" "wasm32-unknown-unknown"

```

Tests were performed on Ubuntu 24.04 with rust 1.82.0. I also tried 1.81.0 and 1.80.0; they fail the same way. `wasm-bindgen` 0.2.95 also fails the same way.
