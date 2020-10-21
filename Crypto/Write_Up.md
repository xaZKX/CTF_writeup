## Crypto

#### MD5

- `题目.txt`：e00cf25ad42683b3df678c61f42c6bda
- 根据提示，这是经过MD5加密的字符串，所以进行解密，即得flag。

#### Url编码

- `题目.txt`：%66%6c%61%67%7b%61%6e%64%20%31%3d%31%7d

- 根据提示，这是经过url编码的字符串，所以进行解码，即得flag。

  ```python
  from urllib import parse
  
  strings = parse.unquote('%66%6c%61%67%7b%61%6e%64%20%31%3d%31%7d') #解码字符串
  print(strings)  
  
  flag{and 1=1}
  ```

#### 一眼就解密

- 根据提示：下面的字符串解密后便能获得flag：ZmxhZ3tUSEVfRkxBR19PRl9USElTX1NUUklOR30=

- 给的字符串是经过base64加密的，解密即得flag。

  ```python
  import base64
  
  s = 'ZmxhZ3tUSEVfRkxBR19PRl9USElTX1NUUklOR30='
  
  print(base64.b64decode(s))
  ```

#### 看我回旋踢

- `题目.txt`：synt{5pq1004q-86n5-46q8-o720-oro5on0417r1}

- 这看起来像是flag经过了凯撒密码加密，尝试进行解密。

- flag的标准格式是：`flag{........}`，故`f`加密为`s`，经过了13的移位。

  ```python
  # -*- coding: utf-8 -*-
  class CaesarCipher(object):
      """
      凯撒加密解密
      """
  
      def __crypt(self, char, key):
          """
          对单个字母加密，偏移
          @param char: {str} 单个字符
          @param key: {num} 偏移量
          @return: {str} 加密后的字符
          """
          if not char.isalpha():
              return char
          else:
              base = "A" if char.isupper() else "a"
              return chr((ord(char) - ord(base) + key) % 26 + ord(base))
  
      def encrypt(self, char, key):
          """
          对字符加密
          """
          return self.__crypt(char, key)
  
      def decrypt(self, char, key):
          """
          对字符解密
          """
          return self.__crypt(char, -key)
  
      def __crypt_text(self, func, text, key):
          """
         对文本加密
         @param char: {str} 文本
         @param key: {num} 偏移量
         @return: {str} 加密后的文本
         """
          lines = []
          for line in text.split("\n"):
              words = []
              for word in line.split(" "):
                  chars = []
                  for char in word:
                      chars.append(func(char, key))
                  words.append("".join(chars))
              lines.append(" ".join(words))
          return "\n".join(lines)
  
      def encrypt_text(self, text, key):
          """
          对文本加密
          """
          return self.__crypt_text(self.encrypt, text, key)
  
      def decrypt_text(self, text, key):
          """
          对文本解密
          """
          return self.__crypt_text(self.decrypt, text, key)
  
  
  if __name__ == '__main__':
      key = 13
      cipher = CaesarCipher()
      
      # 解密
      print(cipher.decrypt_text("synt{5pq1004q-86n5-46q8-o720-oro5on0417r1}", key))
  ```

#### 摩丝

- `题目.txt`：.. .-.. --- ...- . -.-- --- ..-

- 根据提示，这是经过摩斯密码加密的flag，解密即得flag。

- 摩斯密码表：

  ![](images/mosi1.png)

#### [BJDCTF 2nd]签到-y1ng

- 根据提示：给定字符串签到：QkpEe1czbGMwbWVfVDBfQkpEQ1RGfQ==

- 给的字符串是经过base64加密的，解密即得flag。

  ```python
  import base64
  
  s = 'QkpEe1czbGMwbWVfVDBfQkpEQ1RGfQ=='
  
  print(base64.b64decode(s))
  ```

#### password

- 题目：

  ![](images/pass1.png)

- 除了这三行没有其他信息。简单组合「姓名首字母+生日」即得flag。

#### 变异凯撒

- 加密密文：afZ_r9VYfScOeO_UL^RWUc

- 根据提示，此题应该是凯撒密码的加强版。下表是前四位的加密信息，根据这个规则，继续解密。

  | 原文 | 密文 | 平移(ASCII) |
  | :--: | :--: | :---------: |
  |  f   |  a   |     -5      |
  |  l   |  f   |     -6      |
  |  a   |  Z   |     -7      |
  |  g   |  _   |     -8      |

- 在「看我回旋踢」的基础上修改加密方式，解密后即得flag。

  ```python
  # -*- coding: utf-8 -*-
  class CaesarCipher(object):
      """
      凯撒加密解密
      """
  
      def __crypt(self, char, key):
          """
          对单个字母加密，偏移
          @param char: {str} 单个字符
          @param key: {num} 偏移量
          @return: {str} 加密后的字符
          """
          return chr(ord(char) + key)
  
      def encrypt(self, char, key):
          """
          对字符加密
          """
          return self.__crypt(char, -key)
  
      def decrypt(self, char, key):
          """
          对字符解密
          """
          return self.__crypt(char, key)
  
      def __crypt_text(self, func, text, key):
          """
         对文本加密
         @param char: {str} 文本
         @param key: {num} 偏移量
         @return: {str} 加密后的文本
         """
          lines = []
          for line in text.split("\n"):
              words = []
              for word in line.split(" "):
                  chars = []
                  for char in word:
                      chars.append(func(char, key))
                      key += 1
                  words.append("".join(chars))
              lines.append(" ".join(words))
          return "\n".join(lines)
  
      def encrypt_text(self, text, key):
          """
          对文本加密
          """
          return self.__crypt_text(self.encrypt, text, key)
  
      def decrypt_text(self, text, key):
          """
          对文本解密
          """
          return self.__crypt_text(self.decrypt, text, key)
  
  
  if __name__ == '__main__':
      key = 5
  
      cipher = CaesarCipher()
  
      # 解密
      print(cipher.decrypt_text("afZ_r9VYfScOeO_UL^RWUc", key))
  ```

#### Quoted-printable

- `题目.txt`：=E9=82=A3=E4=BD=A0=E4=B9=9F=E5=BE=88=E6=A3=92=E5=93=A6
- 根据提示，这是「可打印字符引用编码」，[解码](http://web.chacuo.net/charsetquotedprintable)即得flag。

#### Rabbit

- `题目.txt`：U2FsdGVkX1/+ydnDPowGbjjJXhZxm2MP2AgI
- 根据提示，这是「Rabbit」加密，解密即得flag。

#### 篱笆墙的影子

- `题目.txt`：felhaagv{ewtehtehfilnakgw}

- 根据提示，这是「栅栏密码」。

  ```
  flag{wethinkw
  ehavetheflag}
  ```

- 两行合并即得flag。

####RSA

- `题目.txt`：

  ![](images/rsa1.png)

- 解密：

  
  $$
  p = 473398607161，q = 4511491 \\
  n = p \times q = 2135733555619387051 \\
  φ(n) = (p-1)\times(q-1) = 2135733082216268400 \\
  e = 17 \\
  C ＝ M^e \ mod\ n \\
  M ＝C^d \ mod \ n \\
  e \times d \ mod \ φ(n) = 1
  $$

  ```python
  import gmpy2
  
  def Decrypt(c,e,p,q):
  	L=(p-1)*(q-1)
  	d=gmpy2.invert(e,L)
  	n=p*q
  	m=gmpy2.powmod(c,d,n)
  	flag=str(d)
  	print("flag{"+flag+"}")
  
  if __name__ == '__main__':
  	p=473398607161
  	q=4511491
  	e=17
  	c=55
  	Decrypt(c,e,p,q)
  ```

####丢失的MD5

#### [BJDCTF 2nd]老文盲了 

#### Alice与Bob

