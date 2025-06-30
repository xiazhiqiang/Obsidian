### "spawn npx ENOENT spawn npx ENOENT"

解决方案：
https://github.com/modelcontextprotocol/servers/issues/64

示例：

```json
{
	"mcpServers": {
		"mastergo-magic-mcp": {
			"disabled": false,
			"timeout": 60,
			"command": "/Users/chengmu/.nvm/versions/node/v18.20.8/bin/node",
			"args": [
				"/Users/chengmu/.nvm/versions/node/v18.20.8/lib/node_modules/@mastergo/magic-mcp/dist/index.js",
				"--url=https://mastergo.com"
			],
			"env": {
				"NPM_CONFIG_REGISTRY": "https://registry.npmmirror.com/"
			},
			"transportType": "stdio"
		}
	}
}
```

### mastergo mcp server配置

https://mcp.so/server/mastergo-magic-mcp/mastergo-design

```json
{
  "mcpServers": {
    "mastergo-magic-mcp": {
      "command": "npx",
      "args": [
        "-y",
        "@mastergo/magic-mcp",
        "--token=TOKEN",
        "--url=https://mastergo.com"
      ],
      "env": {
        "TOKEN": "mg_3ede54387cf04bb3ac51fc2e52cead77"
      }
    }
  }
}
```
