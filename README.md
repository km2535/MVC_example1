# JAVA-17
## 파일 입출력(출력)
* Reader(출력)
  * FileReader -> BufferedReader -> 파일에서 출력
  * FileReader
    * 경로에 있는 파일 준비 시키기(읽기 위해)
    * 파일이 없으면 예외 발생 -> try/catch문으로
  * BufferedReader
    * 버퍼를 이용해서 파일 일기
  * .readLine()
    * \r\n을 기준으로 한 줄씩 읽어오기 
파일 읽어오기 예제
```java
package io;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;

public class ReaderTest {
	public static void main(String[] args) {
		ArrayList<String> datas =  new ArrayList<String>();
		try {
//		파일을 읽기 위해서는 먼저 FileReader를 만들어준다.
			FileReader fr = new FileReader("C:\\JAVA_WEB_LKM\\java\\workspace\\day17\\lang.txt");
			System.out.println("파일 준비완료");
//		경로가 없을 수도 있고 파일을 읽는데 실패할 수 도 있으니 trycatch를 하지 않으면 에러발생,
//		자동완성을 시켜보면 trycatch문으로 만들어 준다.
//		다음으로 버퍼를 준비한다.
			BufferedReader br = new BufferedReader(fr);
			System.out.println("버퍼 준비완료");
//		파일을 읽어 오려면 다음과 같이 파일을 반복문을 돌려서 읽을 수 있다.
			while(true) {
//				또한 파일을 읽어 보는데 실패 할 수도 있으니 catch문을 추가한다.
				String msg = br.readLine();
				if(msg == null) {
					break;
				}
//				만든 데이터를 msg들을 add하여 배열로 만든다.
				datas.add(msg);
				//System.out.println(msg);
//				파일을 이렇게 하나씩 가져올 수 도 있지만 collection을 이용해서 배열화 할 수 있다.
			}
			System.out.println(datas);
		} catch (FileNotFoundException e) {
			System.out.println("존재하지 않는 파일");
		} catch(IOException e) {
			System.out.println("파일 읽기 실패");
		}
	}
}

```
## MVC
소프트웨어 설계시 사용되는 디자인 패턴
만든는 방법이 아닌 **잘** 만드는 방법에 대해 배우는 것
M : Model(데이터와 반응)
V : View(보여지는 화면)
C : Controller(흐름제어)
* MVC Model1
  * 사이즈가 작고 간단한 프로젝트에 어울림
  * View, Controller가 함께 공존하는 형태 
  * 아래 코드는 view를 담당하는 코드와 함께 처리(controller)도 공존한다. 
```java
package view;

import java.util.Scanner;

//index : 시작하는 페이지
public class Index {
	public static void main(String[] args) {
		System.out.println("22.03 개강반 최종 프로젝트 / UMS 프로그램 입니다.");
		
		Scanner sc = new Scanner(System.in);
		while(true) {
			System.out.println("1. 회원가입\n2. 로그인\n3. 나가기");
			int choice = sc.nextInt();
			
			//Controller
			if(choice == 3) {
				System.out.println("안녕히가세요");
				break;
			}
			switch(choice) {
			case 1:
				//회원가입
				new JoinView();
				break;
			case 2:
				//로그인
				new LoginView();
				break;
			default:
				System.out.println("다시 입력하세요");
			}
		}
	}
}
```

* MVC Model2
  * view, Controller가 완벽하게 분리된 형태 
* DTO(Data Transfer Object) / VO(Value Object)
  * 양쪽으로 전송되어 오고가는 데이터들을 담은 객체
  * 여러 데이터들을 포장해서 만든 데이터 포장용 객체
  * 데이터 전송 객체
  * 실습예제에서는 사용자 정보를 JoinView에서 전달 받아 모두 포장하여 DAO로 넘겨주고 있다.

* DAO(Data Access Object)
  * 저장되어 있는 데이터에 접근하기 위한 객체
  * 데이터들을 관리(추가, 수정, 삭제, 읽기) 하는 메소듣들이 정의 되어 있다.
    * 실습 예제에서는 DBConnection을 만들어 CRUD를 처리한다. 
  * 데이터 접근 객체
    * DBConnection은 각 CRUD를 정의하여 외부 파일과 소통한다.
  > **CRUD**
  Create(생성), Read(읽기), Update(수정), Delete(삭제)의 줄임말,


### 종합예제
#### 종합예제에 사용되는 각 클래스와 패키지의 관계도
![](https://imagedelivery.net/v7-TZByhOiJbNM9RaUdzSA/398fcd36-cb38-43b5-5cf7-bf8a80b6b200/public)
#### 예제 코드
##### view / index
```java
package view;

import java.util.Scanner;

//index : 시작하는 페이지
public class Index {
	public static void main(String[] args) {
		System.out.println("22.03 개강반 최종 프로젝트 / UMS 프로그램 입니다.");
		
		Scanner sc = new Scanner(System.in);
		while(true) {
			System.out.println("1. 회원가입\n2. 로그인\n3. 나가기");
			int choice = sc.nextInt();
			
			//Controller
			if(choice == 3) {
				System.out.println("안녕히가세요");
				break;
			}
			switch(choice) {
			case 1:
				//회원가입
				new JoinView();
				break;
			case 2:
				//로그인
				new LoginView();
				break;
			default:
				System.out.println("다시 입력하세요");
			}
		}
	}
}
```
##### view / JoinView
```java
package view;

import java.util.Scanner;

import dao.UserDAO;
import dto.UserDTO;
// 회원가입 뷰단
public class JoinView {
	public JoinView() {
		Scanner sc = new Scanner(System.in);
		UserDAO udao = new UserDAO();
		
		System.out.print("아이디 : ");
		String userid = sc.next();
        //userid를 생성할때 중복검사 할 도구가 필요하다. 
        //데이터와 통신할 클래스를 별도로 만드는데 DAO로 관리하면 된다.
        // DAO에는 데이터베이스와 직접 통신할 DBConnection Class와 DBConnection 클래스와 직접 소통하는 클래스로 나눌 것이다. 
        // UserDAO에서 만든 메소드를 이용해 사용자 중복 검사를 실시하자
		if(udao.checkDup(userid)) {
			System.out.print("비밀번호 : ");
			String userpw = sc.next();
			System.out.print("이름 : ");
			String username = sc.next();
			System.out.print("나이 : ");
			int userage = sc.nextInt();
			System.out.print("핸드폰 번호 : ");
			String userphone = sc.next();
			System.out.print("주소 : ");
			sc = new Scanner(System.in);
			String useraddr = sc.nextLine();
		
			UserDTO user = new UserDTO(userid, userpw, username, userage, userphone, useraddr);
			if(udao.join(user)) {
				System.out.println("회원가입 성공!");
				System.out.println(username+"님 가입을 환영합니다!");
			}
			else {
				System.out.println("회원가입 실패 / 다시 시도해 주세요.");
			}
		}
		else {
			System.out.println("중복된 아이디가 있습니다. 다시 시도해 주세요.");
		}
	}
}
```
##### dao / UserDAO
```java
package dao;

import java.util.HashSet;

import dto.UserDTO;

public class UserDAO {
	//DBConnction과 소통하기 위해 기본 생성자에 정의해둔다.
	DBConnection conn = null;
	
	public UserDAO() {
    // 이 클래스가 객체화 되면 자동으로 DBConnection과 소통할 것이다. 
    // 클래스는 경로data를 받기로 정의 되어 있다. 
		conn = new DBConnection("database/UserTable.txt");
	}
	// DBConnection클래스 안에는 데이터를 넣는 메소드를 정의했고 메소드명은 insert이다.'
    // 매개변수로 회원가입 시 받는 아이디를 받아서 insert메소드에 전달해주자.
	public boolean join(UserDTO user) {
    //toString도 재정의 하였음. 
		return conn.insert(user.toString());
	}
	//DBConnection클래스 안에 데이터를 가져오는 select메소드가 있다. 
    // 이 메소드 타입은 Set타입으로 HashSet타입을 이용해 가져올 수 있다. 
	public boolean checkDup(String userid) {
		HashSet<String> rs = conn.select(0,userid);
        // 가져온 데이터가 0일 경우 중복되는 데이터가 없다는 것이고 true이다. 
        // 우리는 이 메소드가 true를 리턴하면 회원가입 시키도록 설계할 것이다.
		return rs.size() == 0;
	}
    //로그인 메소드도 같이 정의해주자, 데이터베이스와 소통하는 클래스로 직접 소통은 DBConnection이 하지만
    // 우리는 DBConnection과 연결되도록 정의하면 된다.

	//							apple		 abcd1234
	public boolean login(String userid, String userpw) {
 // DBConnection을 통해 데이터를 가져옴
		HashSet<String> rs = conn.select(0, userid);
		// size가 1이라면 로그인 하려는 아이디가 존재한다는 뜻임.
		if(rs.size() == 1) {
			for(String line : rs) {
            //그 아이디가 담겨있는 데이터 한줄을 가져와서 split하여 비밀번호만 쓴다느 거임
				//line : "apple	abcd1234	김사과	10	01012341234	서울시 강남구 역삼동"
				//"문자열1".split("문자열2") : "문자열1" 을 "문자열2" 기준으로 나누기
				// 						나뉘어진 문자열들이 담겨있는 배열 return
				//{"apple","abcd1234","김사과","10","01012341234","서울시 강남구 역삼동"}
				if(line.split("\t")[1].equals(userpw)) {
                //줄을 가져와서 split을 통해 비밀번호를 가져오고  Session에 로그인 값을 저장한다음 로그인 성공을 리턴한다.
					//로그인 성공
//					Session.datas.put("login_id",userid);
					Session.put("login_id", userid);
					return true;
				}
				
			}
		}
        // 아이디 정보가 없을때, 비밀번호 안 맞을때 false;
		return false;
	}
}
```
##### DBConnection
```java
//여기서 정의된 insert, update, delete, select를 통해 데이터베이스를 다룬다. 
package dao;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashSet;

public class DBConnection {
	String file;
	
	public DBConnection(String file) {
		this.file = file;
	}
	
	boolean insert(String data) {
		try {
			BufferedWriter bw = new BufferedWriter(new FileWriter(file,true));
			bw.write(data+"\r\n");
			bw.close();
			return true;
		} catch (IOException e) {
			System.out.println("======오류 발생 : DB 연결 실패======");
			System.out.println(e);
			System.out.println("===============================");
		}
		return false;
	}
	boolean update(String key,int col,String newData) {
		String result = "";
		boolean check = false;
		try {
			BufferedReader br =  new BufferedReader(new FileReader(file));
			while(true) {
				String line = br.readLine();
				if(line ==null) {
					break;
				}
				String[] datas=line.split("\t");
				if(datas[0].equals(key)) {
					result+=datas[0];
					for (int i = 1; i < datas.length; i++) {
						if(i==col) {
							result+="\t"+newData;
							check = true;
						}else {
							result+="\t"+datas[i];
						}
					}
					result+="\r\n";
				}else {
					result+=line+"\r\n";
				}
			}
			
		} catch (FileNotFoundException e) {
			System.out.println("======오류 발생 : DB 파일 오류======");
			System.out.println(e);
			System.out.println("===============================");
		} catch (IOException e) {
			System.out.println("======오류 발생 : DB 연결 실패======");
			System.out.println(e);
			System.out.println("===============================");
		}	
		if(check) {
			try {
				BufferedWriter bw = new BufferedWriter(new FileWriter(file));
				bw.write(result);
				bw.close();
			} catch (IOException e) {
				System.out.println("======오류 발생 : DB 연결 실패======");
				System.out.println(e);
				System.out.println("===============================");
			}
		}
		return check;
	}
	boolean delete(String key) {
		String result = "";
		boolean check = false;
		try {
			BufferedReader br =  new BufferedReader(new FileReader(file));
			while(true) {
				String line = br.readLine();
				if(line ==null) {
					break;
				}
				String[] datas=line.split("\t");
				if(datas[0].equals(key)) {
					check = true;				
				}else {
					result+=line+"\r\n";
				}
			}
			
		}catch (FileNotFoundException e) {
			System.out.println("======오류 발생 : DB 파일 오류======");
			System.out.println(e);
			System.out.println("===============================");
		} catch (IOException e) {
			System.out.println("======오류 발생 : DB 연결 실패======");
			System.out.println(e);
			System.out.println("===============================");
		}	
		if(check) {
			try {
				BufferedWriter bw = new BufferedWriter(new FileWriter(file));
				bw.write(result);
				bw.close();
			} catch (IOException e) {
				System.out.println("======오류 발생 : DB 연결 실패======");
				System.out.println(e);
				System.out.println("===============================");
			}
		}
		return check;
	}
	//"file" 안에 있는 모든 정보 검색
	HashSet<String> select() {
		HashSet<String> resultSet = new HashSet<>();
		try {
			BufferedReader br = new BufferedReader(new FileReader(file));
			while(true) {
				String line = br.readLine();
				if(line==null) {
					break;
				}
				resultSet.add(line);
			}
		} catch (FileNotFoundException e) {
			System.out.println("======오류 발생 : DB 파일 오류======");
			System.out.println(e);
			System.out.println("===============================");
		} catch (IOException e) {
			System.out.println("======오류 발생 : DB 연결 실패======");
			System.out.println(e);
			System.out.println("===============================");
		}	
		return resultSet;
	}
	HashSet<String> select(int col,String data) {
		HashSet<String> resultSet = new HashSet<>();
		try {
			BufferedReader br = new BufferedReader(new FileReader(file));
			while(true) {
				String line = br.readLine();
				if(line==null) {
					break;
				}
				String[] datas = line.split("\t");
				if(datas[col].equals(data)) {
					resultSet.add(line);//apple	abcd1234	김사과	10	01012341234	서울시 강남구 역삼동
				}
			}
		} catch (FileNotFoundException e) {
			System.out.println("======오류 발생 : DB 파일 오류======");
			System.out.println(e);
			System.out.println("===============================");
		} catch (IOException e) {
			System.out.println("======오류 발생 : DB 연결 실패======");
			System.out.println(e);
			System.out.println("===============================");
		}	
		return resultSet;
	}
	String lastPK() {
		String pk = null;
		try {
			BufferedReader br = new BufferedReader(new FileReader(file));
			while(true) {
				String line = br.readLine();
				if(line==null) {
					break;
				}
				pk=line.split("\t")[0];
			}
		} catch (FileNotFoundException e) {
			System.out.println("======오류 발생 : DB 파일 오류======");
			System.out.println(e);
			System.out.println("===============================");
		} catch (IOException e) {
			System.out.println("======오류 발생 : DB 연결 실패======");
			System.out.println(e);
			System.out.println("===============================");
		}
		return pk;
	}
}
```
##### dao / Session
```java
package dao;

import java.util.HashMap;

public class Session {
//로그인이 완료된 값들(userid)를 받아서 datas에 저장하고 관리한다는 거임.
	//Session.datas.put("login_id","apple");
    //물론 이 데이터들은 밖에서 아무데나 사용하면 안됨으로 private로 만들었다.
	private static HashMap<String, String> datas = new HashMap<String, String>();

	// put메소드를 정의해주고 UserDAO에서  아래 주석과 같이 사용하게 된다. 
	//Session.put("login_id","apple");
	public static void put(String key, String value) {
    //아래 데이터와 같이 쌍으로 저장될 것이다. 
		datas.put(key, value);
	}
	public static String get(String key) {
		return datas.get(key);
	}
}
```
##### dto / UserDTO
회원가입을 하고 나면 많은 정보들을 처리할 곳이 필요하다. 회원가입 클래스에서 데이터를 받고
이 데이터를 저장하거나 관리하려면 DAO로 전달해주어야 한다.
```jave
package dto;

public class UserDTO {
	//Alt + Shift + A : 그리드 편집 모드(여러줄 동시에 편집)
   // 회원가입을 통해 받은 변수들이다. 
	public String userid;
	public String userpw;
	public String username;
	public int userage;
	public String userphone;
	public String useraddr;
	//아래와 같이 회원가입 view창에서 객체화를 시켜 값을 담았다. 
	public UserDTO(String userid, String userpw, String username, int userage, String userphone, String useraddr) {
		this.userid = userid;
		this.userpw = userpw;
		this.username = username;
		this.userage = userage;
		this.userphone = userphone;
		this.useraddr = useraddr;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof UserDTO) {
			UserDTO target = (UserDTO)obj;
			
			if(target.userid.equals(this.userid)) {
				return true;
			}
		}
		return false;
	}
	
	@Override
	public String toString() {
		//apple	abcd1234	김사과	10	01012341234	서울시 강남구 역삼동
		return userid+"\t"+userpw+"\t"+username+
				"\t"+userage+"\t"+userphone+"\t"+useraddr;
	}
}
```
물론 여기서 바로 처리되지 않고 JoinView 클래스에서 객체화되고 그 객체가 데이터를 가지면 
DAO에 전달해주게 된다. 
전달 받은 데이터는 메소드를 통해 받고 이 메소드는 다시 DBConnection을 통해 데이터베이스에 저장되게 된다.

##### view / LoginView
마찬가지로 여기서도 UserDAO를 객체화 하여 사용한다. 사용자로부터 받은 데이터를 dao가 가지는 메소드를 통해 전달하게 되고 데이터를 전달받은 dao는 DBConnection을 통해 저장된 데이터베이스에 접근하여 값을 가져오고 userid를 비교하여 성공여부를 판단, 성공 시 Session에 로그인 정보를 전달한다. 
```java
package view;

import java.util.Scanner;

import dao.UserDAO;

public class LoginView {
	public LoginView() {
		Scanner sc = new Scanner(System.in);
		UserDAO udao = new UserDAO();
		
		System.out.print("아이디 : ");
		String userid = sc.next();
		System.out.print("비밀번호 : ");
		String userpw = sc.next();
		
		if(udao.login(userid,userpw)) {
			System.out.println(userid+"님 어서오세요~");
			
			//메인창 띄우기
			new MainView();
		}
		else {
			System.out.println("로그인 실패 / 다시 시도해 주세요.");
		}
	}
}
```
성공 시 메인창으로 가도록 코드가 작성되었다. 

##### view / MainView
아직 구현을 다하지 않음... 
session에는 영구적으로 userid가 저장된것은 아니다. 프로그램이 실행 될때마다 
데이터베이스에서 select메소드를 통해 가져오도록 설계되어 있는 것이다. 
session에 저장된 userid를 가져와서 사용자 정보를 보여주고 있다. 
```java
package view;

import java.util.Scanner;

import dao.Session;

public class MainView {
	public MainView() {
		Scanner sc = new Scanner(System.in);
//		세션에서 로그인 정보를 가져옴.
		String userid = Session.get("login_id");
		
		while(true) {
			System.out.println("☆★☆★☆★☆★"+userid+"님 어서오세요~☆★☆★☆★☆★\n"
					+ "1. 상품추가\n2. 상품수정\n3. 상품삭제\n"
					+ "4. 내 상품 보기\n5. 상품 검색\n6. 내 정보 수정\n7. 로그아웃");
			int choice = sc.nextInt();
			switch(choice) {
			case 1: 
				break;
			case 2: 
				break;
			case 3: 
				break;
			case 4: 
				break;
			case 5: 
				break;
			case 6: 
				break;
			case 7: 
				break;
			}
		}
	}
}
```