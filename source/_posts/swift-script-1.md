---
title: Swift脚本-执行Shell命令
date: 2020-11-06 11:13:48
category:
- Software
- Swift
tags:
---

Swift脚本之执行Shell命令

<!--more-->

完整的代码如下

```swift
#!/usr/bin/env swift
import Foundation

@dynamicCallable
@dynamicMemberLookup
class Command {
    
    private lazy var output: Pipe = {
        return Pipe()
    }()
    private var components: [String] = []
    private var rendered: [String] {
        return components
    }
    
    init(components: [String]) {
        self.components = components
    }

    func run(isLog log: Bool = true, standardInput: Any? = nil, standardOutput: Any? = nil, standardError: Any? = nil, currentDirectory: String? = nil) throws {
        var isLog = log
        if (standardOutput != nil || standardError != nil) {
            isLog = false
        }
        
        let process = Process()
        process.currentDirectoryURL = URL(fileURLWithPath: currentDirectory ?? "")
        process.executableURL = URL(fileURLWithPath: "/usr/bin/env")
        process.arguments = self.rendered
        process.standardInput = standardInput
        process.standardError = standardError ?? output
        process.standardOutput = standardOutput ?? output
        try process.run()
        if (isLog) {
            let data = output.fileHandleForReading.readDataToEndOfFile()
            print(String(data: data, encoding: .utf8) ?? "LogEmpty")
        }
    }
    
    @discardableResult
    func dynamicallyCall(withArguments args:[String]) -> Command {
        self.components.append(contentsOf: args)
        return self
    }
    
    @discardableResult
    func dynamicallyCall(withKeywordArguments args:[String: String?]) -> Command {
        var tmpComponents: [String] = []
        for (var key, valueOp) in args {
            key = key.replacingOccurrences(of: "_", with: "-")
            guard let value = valueOp else {
                tmpComponents.append(key)
                continue
            }
            tmpComponents.append(contentsOf: [key, value])
        }
        self.components.append(contentsOf: tmpComponents)
        return self
    }
    
    static subscript(dynamicMember member: String) -> Command {
        return Command(components: [member])
    }
    
    subscript(dynamicMember member: String) -> Command {
        self.components.append(member)
        return self
    }
}


// Demo::

try Command.ls.run()
try Command.pwd.run()
try Command.pwd.run(currentDirectory: "/Users")
// 当前目录下的aaa文件夹内
try Command.curl("www.baidu.com")(__output: "index.html").run(currentDirectory: "aaa")
```