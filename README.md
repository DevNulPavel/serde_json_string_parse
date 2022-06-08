# Serde json string parse

Provides `ParseJson` trait and `JsonParseError` struct which allow parse `String` or `&str` types to struct using `parse_json_with_data_err` trait method.

In case of error `JsonParseError` contains original `String` or `&str` and original parsing error from serde.

# Examples

```rust
use serde_json_string_parse::{JsonParseError, ParseJson};

#[derive(Deserialize, Debug)]
struct TestStruct {
    key: String,
}

#[rustfmt::skip]
let text = String::from(r#"{
    "key": "value"
}"#);

let parse_result: TestStruct = text.parse_json_with_data_err().expect("Parsing failed");
assert_eq!(parse_result.key, "value");
```

```rust
use serde_json_string_parse::{JsonParseError, ParseJson};

#[derive(Deserialize, Debug)]
struct TestStruct {
    key: String,
}

#[rustfmt::skip]
let text = String::from(r#"{
    "key" ___ "value"
}"#);

let parse_error: JsonParseError<String> = text
    .clone()
    .parse_json_with_data_err::<TestStruct>()
    .expect_err("Parsing must fail");

// `original_data` field contains source text
assert_eq!(parse_error.original_data, text);
```