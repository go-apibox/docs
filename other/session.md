Session类库
============

APIBox 的 Session 类库是基于 `gorilla/sessions` 实现的，它使用加密的 Cookie 作为 Session 的存储介质。

 - [Gorilla Session 文档](http://www.gorillatoolkit.org/pkg/sessions)

与 `gorilla/session` 不同的是，`apibox/session` 在会自动管理 Cookie 的加密密钥，将密钥保存在一个密钥文件中，并定期自动更新密钥。

该类库默认 Cookie 有效期为 30 天，session 有效期为 1 小时，并实现了 session 活跃后自动延长有效期。

### 接口参考


#### CookieStore

 - `func DefaultCookieStore() (*CookieStore, error)`
   返回一个默认的 CookieStore，实际为调用：`NewCookieStore(true, "")`。
   全局只有一个默认 CookieStore，多次调用该函数将返回同一个实例。
    
 - `func NewCookieStore(autoUpdate bool, keyPairsFile string) (*CookieStore, error)`
   创建一个 CookieStore 存储，密钥保存在 `keyPairsFile` 指定的密钥文件中，如果 `keyPairsFile` 为空，则密钥保存在内存中。
   如果指定 `autoUpdate` 为 true，则密钥会定期更新，更新周期为30天。
   注意：当密钥文件内容变化时，生成的 CookieStore 对象使用的密钥也会自动更新。
   创建成功后，返回的 CookieStore 对象上可以直接使用与 `gorilla/session` 中  CookieStore 完全一样的接口。
   但 Get() 的行为稍有不同。

 - `func (s *CookieStore) Get(r *http.Request, name string) (*Session, error)`
   返回一个指定名称的 Session。

 - `func (store *CookieStore) GenerateKeyPairs()`
   根据旧的密钥对产生一个新的密钥对（共2组4个密钥），保存在对象中。
   此时尚对 Cookie 解密生效，需调用 `LoadKeyPairs` 后才生效。

 - `func (store *CookieStore) LoadKeyPairs(keyPairs [][]byte)`
   加载指定密钥对，加载后对密钥对生效。

 - `func (store *CookieStore) GetKeyPairs() [][]byte`
    返回当前保存在对象中的密钥对。

#### Session

 - `func (s *Session) Get(key string) (interface{}, error)`
   读取 Session 中的指定 key 的值。

 - `func (s *Session) GetString(key string) (string, error)`
   读取 Session 中的指定 key 的值，值必须为 string 类型，否则返回错误。

 - `func (s *Session) GetBool(key string) (bool, error)`
   读取 Session 中的指定 key 的值，值必须为 bool 类型，否则返回错误。

 - `func (s *Session) GetInt(key string) (int, error)`
   读取 Session 中的指定 key 的值，值必须为 int 类型，否则返回错误。

 - `func (s *Session) GetInt32(key string) (int32, error)`
   读取 Session 中的指定 key 的值，值必须为 int32 类型，否则返回错误。

 - `func (s *Session) GetInt64(key string) (int64, error)`
   读取 Session 中的指定 key 的值，值必须为 int64 类型，否则返回错误。

 - `func (s *Session) GetUint(key string) (uint, error)`
   读取 Session 中的指定 key 的值，值必须为uint 类型，否则返回错误。

 - `func (s *Session) GetUint32(key string) (uint32, error)`
   读取 Session 中的指定 key 的值，值必须为uint32 类型，否则返回错误。

 - `func (s *Session) GetUint64(key string) (uint64, error)`
   读取 Session 中的指定 key 的值，值必须为uint64 类型，否则返回错误。

 - `func (s *Session) Set(key string, val interface{})`
   设置单个 Session 值。

 - `func (s *Session) SetMap(m map[string]interface{})`
   设置多个 Session 值。

 - `func (s *Session) Delete(key string)`
   删除 Session 中的指定值，删除后需要调用 Save() 才会生效。

 - `func (s *Session) Save(w http.ResponseWriter) error`
   保存 Session。
   
 - `func (s *Session) Destroy(w http.ResponseWriter) error`
   销毁整个 Session，无需调用 Save()。

 - 
