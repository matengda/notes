获取数据库密码

/data/data/com.tencent.mm/MicroMsg/${md5_value}/EnMicroMsg.db
以上是聊天数据库文件路径
$md5_value 是 md5('mm' + $uin)



/data/data/com.tencent.mm/shared_prefs/auth_info_key_prefs.xml
name 为 _auth_uin 的值

/data/data/com.tencent.mm/shared_prefs/system_config_prefs.xml
name 为 default_uin 的值是最后一个登录微信账号的uin

/data/data/com.tencent.mm/shared_prefs/app_brand_global_sp.xml
里面存放着所有账号的uin

md5(imei + uin)
取前7位, 小写
如果IMEI中有字符的话，需要将其转变为大写

https://gitee.com/mosmos_admin/EnMicroMsg.git
这个项目里面有 sqlcipher 2 版本



```sqlite
.open EnMicroMsg.db
PRAGMA key='';
PRAGMA cipher_use_hmac=off;
PRAGMA cipher_page_size=1024;
PRAGMA kdf_iter=4000;
ATTACH DATABASE 'MicroMsg.db' AS mm KEY '';
SELECT sqlcipher_export('mm');
DETACH DATABASE mm;
.quit
```





```python
import os

path = r'c:\Users\hd\Desktop\a'

for root, dirs, files in os.walk(path):

    for fn in files:

        if ('thum' in fn):
            os.remove(os.path.join(root, fn))
            continue

        os.rename(os.path.join(root, fn), os.path.join(root, fn + '.jpg'))

```



```python
from pysqlcipher3 import dbapi2 as sqlite
import hashlib
def decrypt( key ):
    conn = sqlite.connect( "EnMicroMsg.db" )
    c = conn.cursor()    
    c.execute( "PRAGMA key = '" + key + "';" )
    c.execute( "PRAGMA cipher_use_hmac = OFF;" )
    c.execute( "PRAGMA cipher_page_size = 1024;" )
    c.execute( "PRAGMA kdf_iter = 4000;" )
    c.execute( "ATTACH DATABASE 'EnMicroMsg-decrypted.db' AS wechatdecrypted KEY '';" )
    c.execute( "SELECT sqlcipher_export( 'wechatdecrypted' );" )
    c.execute( "DETACH DATABASE wechatdecrypted;" )
    c.close()
def generate_key():
    imei = "866666666666666"
    uin = "1919191919"
    key = hashlib.md5( str( imei ).encode("utf8") + str( uin ).encode("utf8")).hexdigest()[ 0:7 ]
    return key
def main():   
    key = generate_key()
    decrypt( key )
main()
```



```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.util.Map;
 
public class MapTest {
 
	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		try {
			FileInputStream file = new FileInputStream("C:/Users/sa/Desktop/systemInfo.cfg");
			ObjectInputStream mObjectInputStream = new ObjectInputStream(file);
			Map map = (Map) mObjectInputStream.readObject();
			System.out.println(map);
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```



```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;
import java.util.HashMap;
public class IMEI {
 public static void main(String[] args) {
  try {
   ObjectInputStream in = new ObjectInputStream(new FileInputStream(
     args[0]));
   Object DL = in.readObject();
   HashMap hashWithOutFormat = (HashMap) DL;
   ObjectInputStream in1 = new ObjectInputStream(new FileInputStream(
     args[1]));
   Object DJ = in1.readObject();
   HashMap hashWithOutFormat1 = (HashMap) DJ;
   String s = String.valueOf(hashWithOutFormat1.get(Integer
     .valueOf(258))); // 取手机的IMEI
   s = s + hashWithOutFormat.get(Integer.valueOf(1)); //合并到一个字符串
   s = encode(s); // hash
   System.out.println("The Key is : " + s.substring(0, 7));
   in.close();
   in1.close();
  } catch (Exception e) {
   e.printStackTrace();
  }
 }
 public static String encode(String content)
  {
   try {
    MessageDigest digest = MessageDigest.getInstance("MD5");
    digest.update(content.getBytes());
    return getEncode32(digest);
    }
   catch (Exception e)
   {
    e.printStackTrace();
   }
   return null;
  }
  private static String getEncode32(MessageDigest digest)
  {
   StringBuilder builder = new StringBuilder();
   for (byte b : digest.digest())
   {
    builder.append(Integer.toHexString((b >> 4) & 0xf));
    builder.append(Integer.toHexString(b & 0xf));
   }
    return builder.toString();
 
  }
}
```



```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.security.MessageDigest;
import java.util.HashMap;
public class GetDBKey {
 public static void main(String[] args) {
  try {
   ObjectInputStream in = new ObjectInputStream(new FileInputStream("CompatibleInfo.cfg"));
   Object DL = in.readObject();
   HashMap hashWithOutFormat = (HashMap) DL;
   String s = String.valueOf(hashWithOutFormat.get(Integer.valueOf(258))); // 取手机的IMEI
   System.out.println("IMEI:"+s);
   ObjectInputStream in1 = new ObjectInputStream(new FileInputStream("systemInfo.cfg"));
   Object DJ = in1.readObject();
   HashMap hashWithOutFormat1 = (HashMap) DJ;
   String t = String.valueOf(hashWithOutFormat1.get(Integer.valueOf(1))); // 取微信的uin
   System.out.println("uin:"+t);
   s = s + t; //合并到一个字符串
   s = encode(s); // MD5
   System.out.println("密码是 : " + s.substring(0, 7));
   in.close();
   in1.close();
  } catch (Exception e) {
   e.printStackTrace();
  }
 }
 public static String encode(String content)
  {
   try {
    MessageDigest digest = MessageDigest.getInstance("MD5");
    digest.update(content.getBytes());
    return getEncode32(digest);
    }
   catch (Exception e)
   {
    e.printStackTrace();
   }
   return null;
  }
  private static String getEncode32(MessageDigest digest)
  {
   StringBuilder builder = new StringBuilder();
   for (byte b : digest.digest())
   {
    builder.append(Integer.toHexString((b >> 4) & 0xf));
    builder.append(Integer.toHexString(b & 0xf));
   }
    return builder.toString();
  
  }
}
```

