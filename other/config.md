配置
=====

配置类通过载入 yaml 格式字符串来生成配置对象，后续可通过该对象对配置进行便携地读取。


## 接口列表

	func FromFile(configFile string) (*Config, error)
	func FromString(yamlStr string) (*Config, error)
    func (c *Config) GetString(key string) (string, error)
    func (c *Config) GetDefaultString(key string, defaultVal string) string
    func (c *Config) GetStringArray(key string) ([]string, error)
    func (c *Config) GetDefaultStringArray(key string, defaultVal []string) []string
    func (c *Config) GetInt(key string) (int, error)
    func (c *Config) GetDefaultInt(key string, defaultVal int) int
    func (c *Config) GetIntArray(key string) ([]int, error)
    func (c *Config) GetDefaultIntArray(key string, defaultVal []int) []int
    func (c *Config) GetBool(key string) (bool, error)
    func (c *Config) GetDefaultBool(key string, defaultVal bool) bool
    func (c *Config) GetBoolArray(key string) ([]bool, error)
    func (c *Config) GetDefaultBoolArray(key string, defaultVal []bool) []bool
    func (c *Config) GetFloat(key string) (float64, error)
    func (c *Config) GetDefaultFloat(key string, defaultVal float64) float64
    func (c *Config) GetFloatArray(key string) ([]float64, error)
    func (c *Config) GetDefaultFloatArray(key string, defaultVal []float64) []float64
    func (c *Config) GetMap(key string) (map[string]interface{}, error)
    func (c *Config) GetDefaultMap(key string, defaultVal map[string]interface{}) map[string]interface{}
    func (c *Config) GetSubKeys(key string) ([]string, error)
    func (c *Config) Len(key string) (int, error)
    func (c *Config) Get(key string) (interface{}, error)

这些函数中的参数 `key` 表示要读取的配置项的名称，多级配置可以用 `.` 号分隔，例如：

 - `app.name`
 - `app.http_addr`

也可以用 `[]` 进行下标索引，如：

 - `web.pages[0]`
 - `web.pages[0].url`

如果配置键名中也存在 `.`，则需要自定义分隔符，可通过 Config 结构的 Delimiter 来指定，如：

	cfg, _ := config.FromFile("config.yaml")
	cfg.Delimiter = "#"
	appName := cfg.GetString("app#name")