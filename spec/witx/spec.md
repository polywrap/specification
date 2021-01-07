# Types
# Modules
## <a href="#w3" name="w3"></a> w3
### Imports
### Functions

---

#### <a href="#_w3_init" name="_w3_init"></a> `_w3_init()`
Init w3

##### Params
##### Results

---

#### <a href="#_w3_invoke" name="_w3_invoke"></a> `_w3_invoke(name_size: u64, args_size: u32)`
Invoke call

##### Params
- <a href="#_w3_invoke.name_size" name="_w3_invoke.name_size"></a> `name_size`: `u64`

- <a href="#_w3_invoke.args_size" name="_w3_invoke.args_size"></a> `args_size`: `u32`

##### Results

---

#### <a href="#__w3_invoke_args" name="__w3_invoke_args"></a> `__w3_invoke_args(name_ptr: s32, args_ptr: s32)`
Get Invoke Arguments

##### Params
- <a href="#__w3_invoke_args.name_ptr" name="__w3_invoke_args.name_ptr"></a> `name_ptr`: `s32`

- <a href="#__w3_invoke_args.args_ptr" name="__w3_invoke_args.args_ptr"></a> `args_ptr`: `s32`

##### Results

---

#### <a href="#__w3_invoke_result" name="__w3_invoke_result"></a> `__w3_invoke_result(ptr: s32, len: u64)`
Set Invoke Arguments

##### Params
- <a href="#__w3_invoke_result.ptr" name="__w3_invoke_result.ptr"></a> `ptr`: `s32`

- <a href="#__w3_invoke_result.len" name="__w3_invoke_result.len"></a> `len`: `u64`

##### Results

---

#### <a href="#__w3_invoke_error" name="__w3_invoke_error"></a> `__w3_invoke_error(ptr: s32, len: u64)`
Set Invoke Error

##### Params
- <a href="#__w3_invoke_error.ptr" name="__w3_invoke_error.ptr"></a> `ptr`: `s32`

- <a href="#__w3_invoke_error.len" name="__w3_invoke_error.len"></a> `len`: `u64`

##### Results

---

#### <a href="#__w3_query" name="__w3_query"></a> `__w3_query(uri_ptr: s32, uri_len: u64, query_ptr: s32, query_len: u64, args_ptr: s32, args_len: u64) -> s8`
Query API

##### Params
- <a href="#__w3_query.uri_ptr" name="__w3_query.uri_ptr"></a> `uri_ptr`: `s32`

- <a href="#__w3_query.uri_len" name="__w3_query.uri_len"></a> `uri_len`: `u64`

- <a href="#__w3_query.query_ptr" name="__w3_query.query_ptr"></a> `query_ptr`: `s32`

- <a href="#__w3_query.query_len" name="__w3_query.query_len"></a> `query_len`: `u64`

- <a href="#__w3_query.args_ptr" name="__w3_query.args_ptr"></a> `args_ptr`: `s32`

- <a href="#__w3_query.args_len" name="__w3_query.args_len"></a> `args_len`: `u64`

##### Results
- <a href="#__w3_query.res" name="__w3_query.res"></a> `res`: `s8`


---

#### <a href="#__w3_query_result_len" name="__w3_query_result_len"></a> `__w3_query_result_len() -> u64`
Query Result

##### Params
##### Results
- <a href="#__w3_query_result_len.res" name="__w3_query_result_len.res"></a> `res`: `u64`


---

#### <a href="#__w3_query_result" name="__w3_query_result"></a> `__w3_query_result(ptr: s32)`
Query Result

##### Params
- <a href="#__w3_query_result.ptr" name="__w3_query_result.ptr"></a> `ptr`: `s32`

##### Results

---

#### <a href="#__w3_query_error_len" name="__w3_query_error_len"></a> `__w3_query_error_len() -> u64`
Query Error

##### Params
##### Results
- <a href="#__w3_query_error_len.res" name="__w3_query_error_len.res"></a> `res`: `u64`


---

#### <a href="#__w3_query_error" name="__w3_query_error"></a> `__w3_query_error(ptr: s32)`
Query Error

##### Params
- <a href="#__w3_query_error.ptr" name="__w3_query_error.ptr"></a> `ptr`: `s32`

##### Results
