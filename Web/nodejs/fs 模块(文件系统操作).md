Node.js 的 `fs` 模块提供了丰富的文件系统操作功能，涉及文件和目录的读取、写入、删除、状态获取、权限管理等。以下是常见操作的详细介绍和示例代码：

### 1. **引入 fs 模块**

```javascript
const fs = require('fs');
```

通过 `require('fs')` 引入文件系统模块，之后就可以使用它提供的各种功能。

---

### 2. **文件读取操作**

#### (1) **异步读取文件**

```javascript
fs.readFile(path, [encoding], callback);
```

- `path`：要读取的文件路径。
- `encoding`：文件编码（如果为 `null`，返回的是 `Buffer` 类型数据）。
- `callback`：回调函数，接收两个参数：`err` 和 `data`。

**示例：**

```javascript
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.log('Error reading file:', err);
    return;
  }
  console.log('File content:', data);
});
```

#### (2) **同步读取文件**

```javascript
const data = fs.readFileSync(path, [encoding]);
```

- 返回文件内容。
- 如果文件较大或者操作较多，建议使用异步方法，避免阻塞事件循环。

**示例：**

```javascript
const data = fs.readFileSync('example.txt', 'utf8');
console.log('File content:', data);
```

---

### 3. **文件写入操作**

#### (1) **异步写入文件**

```javascript
fs.writeFile(path, data, [encoding], callback);
```

- `path`：文件路径。
- `data`：要写入的数据。
- `encoding`：文件编码，默认为 `'utf8'`。
- `callback`：写入完成后的回调函数。

**示例：**

```javascript
fs.writeFile('output.txt', 'Hello, Node.js!', 'utf8', (err) => {
  if (err) {
    console.log('Error writing file:', err);
    return;
  }
  console.log('File written successfully');
});
```

#### (2) **同步写入文件**

```javascript
fs.writeFileSync(path, data, [encoding]);
```

- 在操作较少或不在乎阻塞时使用同步方法。

**示例：**

```javascript
fs.writeFileSync('output.txt', 'Hello, Node.js!', 'utf8');
console.log('File written successfully');
```

#### (3) **追加数据到文件**

- **异步追加**：

```javascript
fs.appendFile(path, data, [encoding], callback);
```

- **同步追加**：

```javascript
fs.appendFileSync(path, data, [encoding]);
```

**示例：**

```javascript
fs.appendFileSync('output.txt', '\nAppended text', 'utf8');
console.log('Data appended successfully');
```

---

### 4. **文件删除操作**

#### (1) **异步删除文件**

```javascript
fs.unlink(path, callback);
```

#### (2) **同步删除文件**

```javascript
fs.unlinkSync(path);
```

**示例：**

```javascript
fs.unlinkSync('output.txt');
console.log('File deleted successfully');
```

---

### 5. **文件状态操作**

#### (1) **异步获取文件状态**

```javascript
fs.stat(path, callback);
```

- 返回一个 `fs.Stats` 对象，包含文件的各种信息。
- `callback(err, stats)`。

**示例：**

```javascript
fs.stat('example.txt', (err, stats) => {
  if (err) {
    console.log('Error getting file stats:', err);
    return;
  }
  console.log('File stats:', stats);
});
```

#### (2) **同步获取文件状态**

```javascript
const stats = fs.statSync(path);
console.log('File stats:', stats);
```

#### `fs.Stats` 常用属性：

- `isFile()`：判断是否为文件。
- `isDirectory()`：判断是否为目录。
- `size`：文件大小（字节）。
- `mtime`：文件的最后修改时间。

---

### 6. **文件重命名操作**

#### (1) **异步重命名文件**

```javascript
fs.rename(oldPath, newPath, callback);
```

#### (2) **同步重命名文件**

```javascript
fs.renameSync(oldPath, newPath);
```

**示例：**

```javascript
fs.renameSync('oldname.txt', 'newname.txt');
console.log('File renamed');
```

---

### 7. **目录操作**

#### (1) **创建目录**

- **异步创建目录**：

```javascript
fs.mkdir(path, [mode], callback);
```

- **同步创建目录**：

```javascript
fs.mkdirSync(path, [mode]);
```

**示例：**

```javascript
fs.mkdirSync('newDirectory');
console.log('Directory created');
```

#### (2) **读取目录内容**

- **异步读取目录内容**：

```javascript
fs.readdir(path, callback);
```

- **同步读取目录内容**：

```javascript
fs.readdirSync(path);
```

**示例：**

```javascript
fs.readdir('someDirectory', (err, files) => {
  if (err) {
    console.log('Error reading directory:', err);
    return;
  }
  console.log('Files in directory:', files);
});
```

#### (3) **删除目录**

- **异步删除目录**：

```javascript
fs.rmdir(path, callback);
```

- **同步删除目录**：

```javascript
fs.rmdirSync(path);
```

---

### 8. **文件流操作**

Node.js 支持流操作，适合处理大文件，避免一次性加载文件内容导致内存占用过多。

#### (1) **创建读取流**

```javascript
const readStream = fs.createReadStream(path, [options]);
```

- `path`：文件路径。
- `options`：可选配置，通常是文件编码和读取文件的大小。

**示例：**

```javascript
const readStream = fs.createReadStream('example.txt', 'utf8');
readStream.on('data', (chunk) => {
  console.log('Reading chunk:', chunk);
});
readStream.on('end', () => {
  console.log('File read complete');
});
```

#### (2) **创建写入流**

```javascript
const writeStream = fs.createWriteStream(path, [options]);
```

**示例：**

```javascript
const writeStream = fs.createWriteStream('output.txt', 'utf8');
writeStream.write('Hello, Node.js!');
writeStream.end();
```

#### (3) **管道操作**

流之间的管道操作允许将一个流的输出直接传递到另一个流的输入。

```javascript
const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt');
readStream.pipe(writeStream);
```

---

### 9. **文件权限操作**

Node.js 的 `fs` 模块还提供了操作文件权限的方法，允许改变文件的访问权限。

#### (1) **更改文件权限**

- **异步更改权限**：

```javascript
fs.chmod(path, mode, callback);
```

- **同步更改权限**：

```javascript
fs.chmodSync(path, mode);
```

#### (2) **更改文件所有者**

- **异步更改文件所有者**：

```javascript
fs.chown(path, uid, gid, callback);
```

- **同步更改文件所有者**：

```javascript
fs.chownSync(path, uid, gid);
```

---

### 10. **其他常见文件操作**

#### (1) **检测文件是否存在**

- **异步检测文件是否存在**：

```javascript
fs.access(path, mode, callback);
```

- **同步检测文件是否存在**：

```javascript
fs.accessSync(path, mode);
```

#### (2) **创建文件描述符**

文件描述符是操作系统为每个打开的文件分配的唯一标识符。

- **打开文件并返回文件描述符**：

```javascript
fs.open(path, flags, [mode], callback);
```

- **关闭文件描述符**：

```javascript
fs.close(fd, callback);
```

#### (3) **读取文件描述符**

```javascript
fs.read(fd, buffer, offset, length, position, callback);
```

---

### 总结

Node.js 提供的 `fs` 模块是操作文件系统的核心工具，它支持多种文件操作方法，包括读取、写入、删除、重命名文件和目录等。对于大文件，Node.js 推荐使用流操作（流是 Node.js 高效处理大数据的方式）。在开发过程中，建议尽量使用异步方法，避免阻塞事件循环，提升程序性能。
