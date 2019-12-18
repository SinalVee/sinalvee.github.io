title: 【翻译】Nodejs - Breaking changes between v5 and v6
date: 2016-05-18 09:10:27
updated: 2016-05-18 09:10:27
categories:
  - NodeJS
tags:
  - nodejs
---

英语水平比较渣，如果有翻译不对的地方请指出，谢谢。  
原文地址：[Nodejs - Breaking changes between v5 and v6](https://github.com/nodejs/wiki-archive/blob/master/Breaking-changes-between-v5-and-v6.md)

---

查看之前的改动日志，请查看[v4 to v5](https://github.com/nodejs/node/wiki/Breaking-changes-between-v4-and-v5)页面。

89个提交被标记为`semver-major`。

<!-- more --> 

**注意**：`#` 与 `.prototype.` 同意，表示该类的实例的属性是可用的。
例如：`Object#toString()` 等同于 `Object.prototype.toString()`。

## By Subsystem
### buffer
[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html)]

- 弃用`new Buffer()`并由一些列新增的[Buffer API: `Buffer.from()`，`Buffer.alloc()`，`Buffer.allocUnsafe()`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_buffer_from_buffer_alloc_and_buffer_allocunsafe)替代。
  - Refs： [`85ab4a5f12`](https://github.com/nodejs/node/commit/85ab4a5f12)，[#4682](https://github.com/nodejs/node/pull/4682)
- 移除之前弃用的`Buffer#write(string, encoding, offset, length)`
  - [`Buffer#write()`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_buf_write_string_offset_length_encoding)保留其他调用方式，如`Buffer#write(string[, encoding])`。
  - Refs: [`2c55cc2d2c`](https://github.com/nodejs/node/commit/2c55cc2d2c),  [#5048](https://github.com/nodejs/node/pull/5048)
- 移除之前弃用的`Buffer#{get|set}`方法。
  - `Buffer#get(index)`由`buffer[index]`替代。
  - `Buffer#set(index, value)`由`buffer[index] = value`替代。
  - Refs: [`101bca988c`](https://github.com/nodejs/node/commit/101bca988c), [#4594](https://github.com/nodejs/node/pull/4594)
- `new Buffer(length, encoding)`现在会抛出异常。
  - 如果传入的`length`参数是数字的话没有什么影响（译者注：个人认为这里应该是传入参数是string类型没有影响，推荐详细阅读PR），这个改动用来指出一种潜在的安全问题。
  - Refs: [`3b27dd5ce1`](https://github.com/nodejs/node/commit/3b27dd5ce1), [#4514](https://github.com/nodejs/node/pull/4514)
- [`SlowBuffer`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_class_slowbuffer)由`Buffer.allocUnsafeSlow()`替代并在文档中给出弃用警告。
  - Refs: [`3fe204c700`](https://github.com/nodejs/node/commit/3fe204c700), [`627524973a`](https://github.com/nodejs/node/commit/627524973a), [#5833](https://github.com/nodejs/node/pull/5833)

### cluster

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/cluster.html)]

- 弃用`Worker#suicide`属性并由语义更清晰的[`Worker#exitedAfterDisconnect`](https://nodejs.org/dist/latest-v6.x/docs/api/cluster.html#cluster_worker_exitedafterdisconnect)替代。
  - 功能保持不变。
  - Refs: [`4f619bde4c`](https://github.com/nodejs/node/commit/4f619bde4c), [#3743](https://github.com/nodejs/node/pull/3743)
- cluster的[`'message'`](https://nodejs.org/dist/latest-v6.x/docs/api/cluster.html#cluster_event_message_1)事件的回调函数现在有三个参数，第一个参数为`worker`。
  - 之前的回调函数的参数为`(message, handle)`，现在是`(worker, message, handle)`。
  - Refs: [`66f4586dd0`](https://github.com/nodejs/node/commit/66f4586dd0), [#5361](https://github.com/nodejs/node/pull/5361)

### console

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/console.html)]

- [`Console#timeEnd(label)`](https://nodejs.org/dist/latest-v6.x/docs/api/console.html#console_console_timeend_label)现在将在完成后清除label。
  - 之前会将label遗留在`_times`中.
  - Refs: [`a5cce79ec3`](https://github.com/nodejs/node/commit/a5cce79ec3), [#3562](https://github.com/nodejs/node/pull/3562)
- 如果label不存在，`Console#timeEnd(label)`现在只会发出警告。
  - 之前会抛出错误。
  - Refs: [`1c84579031`](https://github.com/nodejs/node/commit/1c84579031), [#5901](https://github.com/nodejs/node/pull/5901)

### crypto

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/crypto.html)]

- 改进C++代码中的错误信息的格式。
  - Refs: [`41feaa89e0`](https://github.com/nodejs/node/commit/41feaa89e0), [#3100](https://github.com/nodejs/node/pull/3100), [`1d9451bb5a`](https://github.com/nodejs/node/commit/1d9451bb5a), [#6042](https://github.com/nodejs/node/pull/6042)
- 如果node构建时没有crypto支持，`require('crypto')`会抛出异常。
  - `require('tls')`和`require('https')`同上。
  - Refs: [`f429fe1b88`](https://github.com/nodejs/node/commit/f429fe1b88), [#5611](https://github.com/nodejs/node/pull/5611)
- [`crypto.Certificate`](https://nodejs.org/dist/latest-v6.x/docs/api/crypto.html#crypto_class_certificate)不再有`_handle` 属性。
  - 之前需要`_handle`属性的类方法现在直接调用c++ binding。
  - Refs: [`a37401e061`](https://github.com/nodejs/node/commit/a37401e061), [#5382](https://github.com/nodejs/node/pull/5382)
- [`crypto.pbkdf2()`](https://nodejs.org/dist/latest-v6.x/docs/api/crypto.html#crypto_crypto_pbkdf2_password_salt_iterations_keylen_digest_callback)的`digest`参数现在是必传的。
  - 目前不使用digest参数会打印弃用的警告。
- crypto所有方法的默认编码为`utf8`。
  - 之前默认的编码是`binary` (`latin1`的nodejs版)。
  - Refs: [`b010c87164`](https://github.com/nodejs/node/commit/b010c87164), [#5522](https://github.com/nodejs/node/pull/5522)
- 默认关闭FIPS兼容模式即使node是在FIPS规则下构建。
  - 注意：正常发布的node不是在启用FIPS情况下构建的。
  - Refs: [`7c48cb5601`](https://github.com/nodejs/node/commit/7c48cb5601), [#5181](https://github.com/nodejs/node/pull/5181)

### dgram

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/dgram.html)]

- 如果执行[`Socket#send()`](https://nodejs.org/dist/latest-v6.x/docs/api/dgram.html#dgram_socket_send_msg_offset_length_port_address_callback)没有发生错误，回调函数的`error`参数现在为`null`，而不是`0`.
  - 在io.js 1.0.0中[`c9fd9e2`](https://github.com/nodejs/node/commit/c9fd9e2162)之前就是这样的。
  - Refs: [`4bc1cccb22`](https://github.com/nodejs/node/commit/4bc1cccb22), [#5929](https://github.com/nodejs/node/pull/5929)

### dns

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/dns.html)]

- [`dns.resolve()`](https://nodejs.org/dist/latest-v6.x/docs/api/dns.html#dns_dns_resolve_hostname_rrtype_callback)现在支持解析纯DNS PTR记录。
  - 之前，调用`dns.resolve(hostname, 'PTR', cb)`会调用`dns.reverse()`，之后不再这样。
  - 现在hostname必须传入反转的 *IN-ADDR* 域。
  - Refs: [`dbdbdd4998`](https://github.com/nodejs/node/commit/dbdbdd4998), [#4921](https://github.com/nodejs/node/pull/4921)

Before:

```js
dns.resolve('8.8.4.4', 'PTR', (err, result) => {
  if (err) {
    // handle error
  }
  // result => ['google-public-dns-b.google.com']
});
```

After:

```js
dns.resolve('4.4.8.8-in-addr.arpa', 'PTR', (err, result) => {
  if (err) {
    // handle error
  }
  // result => ['google-public-dns-b.google.com']
});

// one could also simply do
dns.reverse('8.8.4.4', (err, result) => {
  if (err) {
    // handle error
  }
  // result => ['google-public-dns-b.google.com']
});
```

- [`dns.lookupService()`](https://nodejs.org/dist/latest-v6.x/docs/api/dns.html#dns_dns_lookupservice_address_port_callback)现在将port参数强制转换为数字。
  - 之前，如果`port`不是数字会抛出`TypeError`。
  - 现在，如果port不在 0-65535 范围内的话会抛出`TypeError`。
  - Refs: [`f3be421c1c`](https://github.com/nodejs/node/commit/f3be421c1c), [#4883](https://github.com/nodejs/node/pull/4883)

### domain

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/domain.html)]

- 如果没有domain `error`事件处理程序，domains不再将上下文指派给其他错误处理程序。
  - 之前只有在domain的`error`事件被处理的情况下才这样处理。
  - Refs: [`90204cc468`](https://github.com/nodejs/node/commit/90204cc468), [#4659](https://github.com/nodejs/node/pull/4659)

### events

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/events.html)]

- 内部事件处理程序的存储对象`EventEmitter#_events`现在继承自`Object.create(null)`而不是`Object.prototype`。
  - 这样避免了使用原本保留的属性名时的问题，例如：`__proto__`。
  - 这也意味着，模块有意添加在`Object.prototype`上的所有属性在`_events`中都不可用。
  - Refs: [`e38bade828`](https://github.com/nodejs/node/commit/e38bade828), [#6092](https://github.com/nodejs/node/pull/6092)

### freelist

- 移除弃用的`freelist`模块。
  - 这个模块主要用于内部使用，我们没有维护它的打算。
  - 如有需要使用他人的模块会更合适。
  - Refs: [`b70dc67828`](https://github.com/nodejs/node/commit/b70dc67828), [#3738](https://github.com/nodejs/node/pull/3738)

### fs

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html)]

- [`fs.readdir{Sync}()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_readdir_path_options_callback)现在默认返回utf8编码的文件名。
  - 文件名的编码现在通过一个选项来配置。
  - Example: `fs.readdir(path, { encoding: 'hex' }, callback)`
  - Refs: [`060e5f0c00`](https://github.com/nodejs/node/commit/060e5f0c00), [#5616](https://github.com/nodejs/node/pull/5616)
- 弃用重新评估用户代码中的`fs`源码。（译者注：这句不知道怎么翻译才好。原文：Deprecated re-evaluating the fs source code from user code.）
  - Refs: [`1d79787e2e`](https://github.com/nodejs/node/commit/1d79787e2e), [#5102](https://github.com/nodejs/node/pull/5102)
- 弃用[`fs.read()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_read_fd_buffer_offset_length_position_callback)遗留的`(fd, length, position, encoding, callback)`调用方式。
  - Refs: [`1124de2d76`](https://github.com/nodejs/node/commit/1124de2d76), [#4525](https://github.com/nodejs/node/pull/4525)
- `fs.read()`读长为0时不再抛出异常。（译者注：接收数据的buffer长度为0。）
  - Refs: [`2b15e68bbe`](https://github.com/nodejs/node/commit/2b15e68bbe), [#4518](https://github.com/nodejs/node/pull/4518)
- [`fs.link{Sync}()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_link_srcpath_dstpath_callback)现在按照正确的顺序检查调用参数。
  - Refs: [`8b97249893`](https://github.com/nodejs/node/commit/8b97249893), [#3917](https://github.com/nodejs/node/pull/3917)
- [`fs.realpath{Sync}()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_realpath_path_options_callback)现在内部使用`uv_fs_realpath()`而不是JavaScript实现.
  - `cache`参数不再接受对象作为缓存，并且被`options`参数替代。
  - Refs: [`b488b19eaf`](https://github.com/nodejs/node/commit/b488b19eaf), [#3594](https://github.com/nodejs/node/pull/3594)

### globals

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/globals.html)]

- 弃用[`global`](https://nodejs.org/dist/latest-v6.x/docs/api/globals.html#globals_global)的别名`root`和`GLOBAL`。
  - Refs: [`4e46931406`](https://github.com/nodejs/node/commit/4e46931406), [#1838](https://github.com/nodejs/node/pull/1838)

### module

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/modules.html)]

- 现在在相对查找时优先查找当前目录。
  - 之前如果`node_module`目录存在会优先查找`node_module`目录。
  - 例如，之前如果`node_modules/example`存在，`require('./example')`会优先加载`node_modules/example`而不是`./example.js`。
  - Refs: [`d38503ab01`](https://github.com/nodejs/node/commit/d38503ab01), [#5689](https://github.com/nodejs/node/pull/5689)
- 现在使用[`require()`](https://nodejs.org/dist/latest-v6.x/docs/api/globals.html#globals_require)时符号链接会被保留。
  - Refs: [`de1dc0ae2e`](https://github.com/nodejs/node/commit/de1dc0ae2e), [#5950](https://github.com/nodejs/node/pull/5950).
- `require()`的文件中的语法错误现在会打印更多信息。
  - Refs: [`18490d3d5a`](https://github.com/nodejs/node/commit/18490d3d5a), [#4287](https://github.com/nodejs/node/pull/4287)

### net

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/net.html)]

- 现在检查端口有效时更严谨。
  - 现在保证像`true`和`[1]`等值不被视为有效的端口。
  - Refs: [`d0edabecbf`](https://github.com/nodejs/node/commit/d0edabecbf), [#5733](https://github.com/nodejs/node/pull/5733), [`02ac302b6d`](https://github.com/nodejs/node/commit/02ac302b6d), [#5732](https://github.com/nodejs/node/pull/5732)
- [`net.createServer()`](https://nodejs.org/dist/latest-v6.x/docs/api/net.html#net_net_createserver_options_connectionlistener)现在当提供的`options`参数不是对象时会抛出异常。
  - 仍然可以只提供一个connectionListener回调函数。
  - Refs: [`a78b3344f8`](https://github.com/nodejs/node/commit/a78b3344f8), [#2904](https://github.com/nodejs/node/pull/2904)
- `V4MAPPED` DNS hint不再默认设置，但`ADDRCONFIG`仍然会默认设置。
  - 如果你的平台需要设置hints，可以使用[`Socket#connect()`](https://nodejs.org/dist/latest-v6.x/docs/api/net.html#net_socket_connect_options_connectlistener)中新的选项`hints`。
  - Refs: [`b85a50b6da`](https://github.com/nodejs/node/commit/b85a50b6da), [#6021](https://github.com/nodejs/node/pull/6021)

### path

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/path.html)]

- 如果提供的输入不是string类型的话，所有path模块的方法都会抛出异常。
  - Refs: [`08085c49b6`](https://github.com/nodejs/node/commit/08085c49b6), [#5348](https://github.com/nodejs/node/pull/5348)
- 现在[`path.format()`](https://nodejs.org/dist/latest-v6.x/docs/api/path.html#path_path_format_pathobject)在不同平台更一致且更实用。
  - Refs: [`d1000b4137`](https://github.com/nodejs/node/commit/d1000b4137), [#2408](https://github.com/nodejs/node/pull/2408)

### process

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/process.html)]

- 现在使用`process.EventEmitter`会打印弃用警告。
  - 该方法已经在源码中弃用很长时间。
  - Refs: [`25751bedfe`](https://github.com/nodejs/node/commit/25751bedfe), [#5049](https://github.com/nodejs/node/pull/5049)
- 所有之前打印的node警告行为更加一致，现在只通过默认的处理程序发出process [`'warning'`](https://nodejs.org/dist/latest-v6.x/docs/api/process.html#process_event_warning)事件。
  - 包括弃用警告，现在被归类为`DeprecationWarning`.
  - Refs: [`c6656db352`](https://github.com/nodejs/node/commit/c6656db352), [#4782](https://github.com/nodejs/node/pull/4782)
- 现在如果[`process.nextTick()`](https://nodejs.org/dist/latest-v6.x/docs/api/process.html#process_process_nexttick_callback_arg)的参数不是function的话会抛出异常。
  - Refs: [`72e3dd9f43`](https://github.com/nodejs/node/commit/72e3dd9f43), [#3860](https://github.com/nodejs/node/pull/3860)

### querystring

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/querystring.html)]

- [`querystring.parse()`](https://nodejs.org/dist/latest-v6.x/docs/api/querystring.html#querystring_querystring_parse_str_sep_eq_options)返回的解析后的对象现在继承自`Object.create(null)`而不是`Object.prototype`。
  - 这样避免了使用原本保留的属性名时的问题，例如：`__proto__`。
  - 这也意味着，模块有意添加在`Object.prototype`上的所有属性在返回的对象中都不可用。
  - Refs: [`dba245f796`](https://github.com/nodejs/node/commit/dba245f796), [#6055](https://github.com/nodejs/node/pull/6055)
- [`querystring.escape()`](https://nodejs.org/dist/latest-v6.x/docs/api/querystring.html#querystring_querystring_escape)现在对于对象使用`Object#toString()`而不是`Object#valueOf()`。
  - 这样使得它与`encodeURIComponent()`的功能更加一致。
  - [`5dafb435d8`](https://github.com/nodejs/node/commit/5dafb435d8), [#5341](https://github.com/nodejs/node/pull/5341)

### readline

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/readline.html)]

- Readline的历史现在可以通过设置[`createInterface()`](https://nodejs.org/dist/latest-v6.x/docs/api/readline.html#readline_readline_createinterface_options)的选项`historySize`为`0`来禁用。
  - 之前将之设置为`0`会使用默认的`30`行。
  - Refs: [`0303a2552e`](https://github.com/nodejs/node/commit/0303a2552e), [#6352](https://github.com/nodejs/node/pull/6352)
- 不建议使用下面这些不在文档中的readline函数，它们仅用于内部使用：
  - `isFullWidthCodePoint()`, `stripVTControlCharacters()`, `getStringWidth()`, `emitKeys()`
  - Refs: [`ca2e8b292f`](https://github.com/nodejs/node/commit/ca2e8b292f), [#3862](https://github.com/nodejs/node/pull/3862)
- [`Readline#emitKeypressEvents(stream)`](https://nodejs.org/dist/latest-v6.x/docs/api/readline.html#readline_readline_emitkeypressevents_stream)现在总是向提供的stream的`'keypress'`事件传入按键信息。
  - Refs: [`0a62f929da`](https://github.com/nodejs/node/commit/0a62f929da), [#6024](https://github.com/nodejs/node/pull/6024)

### repl

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/repl.html)]

- 现在可以向`_`赋值，`_`通常保存REPL中最后一个表达式的结果。
  - 这样做将会打印一条警告并禁用保存最后一个表达式的结果。
  - Refs: [`ad8257fa5b`](https://github.com/nodejs/node/commit/ad8257fa5b), [5535](https://github.com/nodejs/node/pull/5535)
- 做了一些改进减少当REPL执行失败时的错误数量。
  - Refs: [`3ee68f794f`](https://github.com/nodejs/node/commit/3ee68f794f), [6328](https://github.com/nodejs/node/pull/6328)

### stream

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html)]

- 在使用[object mode](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html#stream_object_mode)时[写入](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html#stream_writable_write_chunk_encoding_callback)一个`null`块现在是无效的，将会抛出`TypeError`。
  - Refs: [`e7c077c610`](https://github.com/nodejs/node/commit/e7c077c610), [#6170](https://github.com/nodejs/node/pull/6170)

### timers

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html)]

- 如果没有向提供`set{`[`Timeout`](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html#timers_settimeout_callback_delay_arg)`|`[`Interval`](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html#timers_setinterval_callback_delay_arg)`|`[`Immediate`](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html#timers_setimmediate_callback_arg)`}()`函数将会立刻抛出异常。
  - 这些方法之前也会抛出异常，但是是在时间到时抛出。
  - Refs: [`ac153bd2a6`](https://github.com/nodejs/node/commit/ac153bd2a6), [#4362](https://github.com/nodejs/node/pull/4362)

### tls

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html)]

- [`tls.Server`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_class_tls_server)的`'clientError'`修改为[`'tlsClientError'`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_event_tlsclienterror)。
  - 因为T`http`现在有[`'clientError'`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_event_clienterror)。
  - Refs: [`1ab6b21360`](https://github.com/nodejs/node/commit/1ab6b21360), [#4557](https://github.com/nodejs/node/pull/4557)
- [`tls.createServer()`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_tls_createserver_options_secureconnectionlistener)'s `sessionIdContext`现在默认使用sha1代替md5进行hash.
  - 仅适用于`sessionIdContext`没有手动设置并且`requestCert`设置为`true`。
  - Refs: [`df268f97bc`](https://github.com/nodejs/node/commit/df268f97bc), [#3866](https://github.com/nodejs/node/pull/3866)
- 在文档中弃用[`tls.createSecurePair()`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_tls_createsecurepair_context_isserver_requestcert_rejectunauthorized_options)并使用[`TLSSocket`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_class_tls_tlssocket)替代。
  - Refs: [`31de5cc436`](https://github.com/nodejs/node/commit/31de5cc436), [#6063](https://github.com/nodejs/node/pull/6063)

### tty

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/tty.html)]

- 移除之前弃用的全局方法`tty.setRawMode()`。
  - 使用[tty instance method](https://nodejs.org/dist/latest-v6.x/docs/api/tty.html#tty_rs_setrawmode_mode)替代。
  - Refs: [`a2c0aa84e0`](https://github.com/nodejs/node/commit/a2c0aa84e0), [#2528](https://github.com/nodejs/node/pull/2528)

### url

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/url.html)]

- 现在如果host改变，[`url.resolve()`](https://nodejs.org/dist/latest-v6.x/docs/api/url.html#url_url_resolve_from_to)将清除认证信息。
  - 这是一种安全措施，以确保身份验证凭据不会泄露。
  - Refs: [`eb4201f07a`](https://github.com/nodejs/node/commit/eb4201f07a), [#1480](https://github.com/nodejs/node/pull/1480)

### util

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/util.html)]

- Error子类现在被格式化为`[MyError: message]`而不是`MyError {}`。
  - Refs: [`e2f47f5698`](https://github.com/nodejs/node/commit/e2f47f5698), [#4582](https://github.com/nodejs/node/pull/4582)
- `Date`现在总是使用`Date#toISOString()`来格式化。
  - Refs: [`93d6b5fb68`](https://github.com/nodejs/node/commit/93d6b5fb68), [#4318](https://github.com/nodejs/node/pull/4318)
- [`util.inspect()`](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_util_inspect_object_options)现在使用c++ bindings检测。
  - Refs: [`24012a879d`](https://github.com/nodejs/node/commit/24012a879d), [#4098](https://github.com/nodejs/node/pull/4098)
- 移除之前弃用的`util.pump()`，请使用[`ReadableStream#pipe()`](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html#stream_readable_pipe_destination_options)替代。
  - Refs: [`007cfea308`](https://github.com/nodejs/node/commit/007cfea308), [#2531](https://github.com/nodejs/node/pull/2531)
- 移除之前弃用的`util.exec()`，请使用[`child_process.exec()`](https://nodejs.org/dist/latest-v6.x/docs/api/child_process.html#child_process_child_process_exec_command_options_callback)替代。
  - Refs: [`4cf19ad1bb`](https://github.com/nodejs/node/commit/4cf19ad1bb), [#2530](https://github.com/nodejs/node/pull/2530)
- TypedArrays格式化现在与普通数组一样。
  - 同样适用于ArrayBuffer和DataView。
  - Refs: [`34a35919e1`](https://github.com/nodejs/node/commit/34a35919e1), [#3793](https://github.com/nodejs/node/pull/3793)
- 弃用[`util._extend()`](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_util_extend_obj)并由[`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)替代。
  - Refs: [`d8290286b3`](https://github.com/nodejs/node/commit/d8290286b3), [#4903](https://github.com/nodejs/node/pull/4903)
- [`util.log()`](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_util_log_string)在文档中给出弃用警告。
  - Refs: [`236b7e8dd1`](https://github.com/nodejs/node/commit/236b7e8dd1), [#6161](https://github.com/nodejs/node/pull/6161)

### vm

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/vm.html)]

- [`vm.Script`](https://nodejs.org/dist/latest-v6.x/docs/api/vm.html#vm_class_script)的`displayErrors`选项现在错误堆栈附加导致错误的代码行。
  - Refs: [`57003520f8`](https://github.com/nodejs/node/commit/57003520f8), [#4874](https://github.com/nodejs/node/pull/4874)

### zlib

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/zlib.html)]

- zlib实例的`close`事件不再在同步调用中发出。
  - 只会影响所有的`*Sync()`方法。
  - Refs: [`8b43d3f52d`](https://github.com/nodejs/node/commit/8b43d3f52d), [#5707](https://github.com/nodejs/node/pull/5707)
- Gzip流的尾随垃圾不再丢弃，现在会抛出错误。
  - 注意：空字节田中不受影响，因为诸多场合已经指出这种填充是正常的，会被`gzip(1)`丢弃。
  - Refs: [`54a5287e3e`](https://github.com/nodejs/node/commit/54a5287e3e), [#5883](https://github.com/nodejs/node/pull/5883)

## Native Modules (Addons)

- ABI模块由于一个模块初始化的次版本增加已经改变。
  - 这仅仅意味着原生插件需要重新编译。
  - Refs: [`71470a8e45`](https://github.com/nodejs/node/commit/71470a8e45), [#4771](https://github.com/nodejs/node/pull/4771)
- `NODE_MODULE_VERSION`现在是`48`。
- 移除一些之前弃用的内部函数。
  - Refs: [`757fbac64b`](https://github.com/nodejs/node/commit/757fbac64b), [#6053](https://github.com/nodejs/node/pull/6053)

## General Node

- 内部工具不再打入node包中，减少了大约10%的体积。
  - Refs: [`90a5fc20be`](https://github.com/nodejs/node/commit/90a5fc20be), [#5695](https://github.com/nodejs/node/pull/5695)
- 现在打印的所有警告以`(node:pid)`开头。
  - Refs: [`d01eb6882f`](https://github.com/nodejs/node/commit/d01eb6882f), [#3878](https://github.com/nodejs/node/pull/3878), [`94b9948d63`](https://github.com/nodejs/node/commit/94b9948d63), [#3833](https://github.com/nodejs/node/pull/3833)
- 现在所有模块的错误消息更加一致。
  - 全部由大写字母开头，除此之外不含存在大写字母的普通单词，不含结尾标识。
  - 另外，参数名和其他代码现在总是被双引号(`\"`)包围。
  - 某些情况下，错误会提供更多信息。
  - Refs: [`20285ad177`](https://github.com/nodejs/node/commit/20285ad177), [#3374](https://github.com/nodejs/node/pull/3374), [`53a95a5b12`](https://github.com/nodejs/node/commit/53a95a5b12), [#5616](https://github.com/nodejs/node/pull/5616), [`8bb60e3c8d`](https://github.com/nodejs/node/commit/8bb60e3c8d), [#5590](https://github.com/nodejs/node/pull/5590), [`ec49fc8229`](https://github.com/nodejs/node/commit/ec49fc8229), [#5981](https://github.com/nodejs/node/pull/5981)
- Node.js不再支持Windows Vista和之前的版本,不会在这些版本上运行。
  - 另外，安装程序在这些系统上不会安装。
  - 现在支持的windows最低版本是Windows 7和Windows Server 2008 R2。
  - Refs: [`1cf26c036d`](https://github.com/nodejs/node/commit/1cf26c036d), [`55db19074d`](https://github.com/nodejs/node/commit/55db19074d), [#5167](https://github.com/nodejs/node/pull/5167)
- Node.js不再支持OS X 10.7一下版本。
  - Refs: [`204f3a8a0b`](https://github.com/nodejs/node/commit/204f3a8a0b), [#6402](https://github.com/nodejs/node/pull/6402)
- 通过`Makefile` (`tools/install.py`)安装不再尝试改变npm的shebang中的node目标路径为本地构建的路径。
  - 保持其为`#!/usr/bin/env node`，即在全局查找node。
  - Refs: [`8ffa20c495`](https://github.com/nodejs/node/commit/8ffa20c495), [#6098](https://github.com/nodejs/node/pull/6098)

## Dependencies

- 现在又支持共享的c-ares builds了。
  - Refs: [`2253be95d0`](https://github.com/nodejs/node/commit/2253be95d0), [#5775](https://github.com/nodejs/node/pull/5775)
- 升级V8至5.0.71.32 + 浮动补丁。
  - Refs: [deps/v8](https://github.com/nodejs/node/commits/master/deps/v8)