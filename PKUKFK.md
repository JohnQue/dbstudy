---


---

<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>
<h1 id="제약조건">제약조건</h1>
<p>물리적인 제약<br>
<strong>제약조건</strong>을 설정하는 이유는 데이터의 <strong>무결성</strong>을 보장하기 위함<br>
<strong>무결성</strong>을 보장하는 것은 무엇이냐?<br>
데이터의 <strong>일관성</strong>, <strong>유효성</strong>, <strong>정확성</strong> 보장<br>
이상한 데이터가 들어오지 않게 제약을 가하겠다.</p>
<h2 id="pk-fk-uk">PK, FK, UK</h2>
<h3 id="pk-primary-key">PK Primary Key</h3>
<p>모델링 관점에서 하나의 테이블 =&gt; 즉 엔티티에는 반드시  <strong>주식별자</strong>가 존재해야 한다.<br>
EX) <strong>학생</strong> 테이블의 <strong>학번</strong><br>
EX) <strong>사원</strong> 테이블의 <strong>사번</strong><br>
EX) <strong>회원</strong> 테이블의 <strong>ID</strong><br>
<strong>주식별자</strong>에 해당 -&gt; 변하지 않고 중복되지 않아야 한다.</p>
<p><strong>주식별자</strong>에 물리적 제약을 두는 것이 바로 <strong>PK</strong><br>
<strong>주식별자</strong>와 <strong>PK</strong>는 다른 의미<br>
<strong>주식별자 = 논리적, PK = 물리적</strong></p>
<p>그래서 <strong>하나의 <code>테이블(엔티티)</code>에는 반드시 <code>주식별자</code>가 존재해야 하지만<br>
<code>PK</code>는 존재할 수도 있고, 안할 수도 있다.</strong></p>
<p>기숙사 =&gt; 통금 12시일때 논리적은 12시가 통금시간 =&gt;<br>
물리적으로 12시에는 문을 잠글 수도 있고 잠그지 않을수도 있다.</p>
<p>주식별자와 PK는 100% 일치하지 않을수도 있지만,<br>
일반적인 경우에는 <strong>주식별자를 PK로 설정</strong>한다.</p>
<h3 id="pk의-특징">PK의 특징</h3>
<p>테이블 당 <strong>하나씩만</strong> 설정가능<br>
<strong>중복</strong>과 <strong>NULL</strong>이 허용되지 않음<br>
<strong>PK</strong>로 설정할 수 있는 컬럼이 <strong>두개 이상 존재 가능</strong><br>
=&gt; <strong>학생</strong> 테이블에 <strong>학번</strong>과 <strong>주민번호</strong>, 즉 둘 다 <strong>PK</strong>로 사용가능할 때<br>
=&gt; 좀 더 많이 쓰이고 길이가 짧은 <strong>학번</strong> 컬럼을 <strong>PK</strong>로 지정<br>
=&gt; Unique Index가 자동으로 생성</p>
<p><strong>PK</strong>를 설정하는 명령어</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">CONSTRAINT</span> PK이름 <span class="token keyword">PRIMARY</span> <span class="token keyword">KEY</span><span class="token punctuation">(</span>PK컬럼<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>ex)</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> PARENT <span class="token keyword">ADD</span> <span class="token keyword">CONSTRAINT</span> P_PK PRIMARY_KEY<span class="token punctuation">(</span>P_ID<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="uk-unique-key">UK Unique Key</h3>
<p><strong>고유키</strong>라고 불림<br>
테이블 당 여러개 설정 가능<br>
중복은 허용되지 않지만 <strong>NULL은 허용</strong>된다.<br>
<strong>PK</strong>와 마찬가지로 Unique Index가 자동으로 생성됨<br>
<strong>UK</strong>를 설정하는 명령어</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">CONSTRAINT</span> UK이름 <span class="token keyword">UNIQUE</span> <span class="token keyword">KEY</span> <span class="token punctuation">(</span>UK컬럼<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="fk-foreign-key">FK Foreign Key</h3>
<p><strong>외래키</strong>라고 불림<br>
<strong>FK</strong>로 설정된 컬럼이 있다. =&gt; 그것이 참조하는 <strong>부모 테이블</strong>이 있다.<br>
예를 들어 회원 테이블에 있는 회원 등급 컬럼에 FK를 설정해놨다<br>
=&gt; 회원 등급 테이블이 존재함을 의미<br>
=&gt; 회원 등급 테이블에 정의되어 있는 데이터만 회원 테이블에 있는 회원 등급 컬럼에 입력이 될 수 있다.<br>
회원 등급 테이블에 다이아몬드, 골드, 실버 등급이 정의가 되어 있는데<br>
=&gt; 회원 테이블의 회원 등급 컬럼에 패밀리 등급이 들어갈 순 없다. (참조 무결성 위배)<br>
=&gt; 단 NULL값이 들어가는 경우는 있다!</p>
<p><strong>FK</strong>컬럼 설정 전에 당연히 <strong>부모 테이블이 먼저 생성</strong>되어 있어야 하고, <strong>참조 컬럼과 데이터 타입도 반드시 일치</strong>해야 한다.<br>
=&gt; 지키지 않으면 <strong>참조 무결성 위배</strong></p>
<p>부모 테이블에 <strong>참조되는 컬럼</strong>은 <strong>PK</strong>나 <strong>UK</strong>로 설정되어 있어야 함<br>
그렇지 않을 경우에는 오류가 나게 되어있음<br>
<strong>FK</strong>를 설정하는 명령어</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> CHILD <span class="token keyword">ADD</span> <span class="token keyword">CONSTRAINT</span> FK이름 <span class="token keyword">FOREIGN</span> <span class="token keyword">KEY</span> <span class="token punctuation">(</span>FK컬럼<span class="token punctuation">)</span> <span class="token keyword">REFERENCES</span> 부모테이블 <span class="token punctuation">(</span>참조할 컬럼<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>Ex)</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">CREATE</span> <span class="token keyword">TABLE</span> PARENT <span class="token punctuation">(</span>
	P_ID VARCHAR2<span class="token punctuation">(</span><span class="token number">2</span><span class="token punctuation">)</span> <span class="token operator">NOT</span> <span class="token boolean">NULL</span>
<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> PARENT <span class="token keyword">ADD</span> <span class="token keyword">CONSTRAINT</span> P_PK <span class="token keyword">PRIMARY</span> <span class="token keyword">KEY</span><span class="token punctuation">(</span>P_ID<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">CREATE</span> <span class="token keyword">TABLE</span> CHILD <span class="token punctuation">(</span>
	C_ID VARCHAR2<span class="token punctuation">(</span><span class="token number">2</span><span class="token punctuation">)</span> <span class="token operator">NOT</span> <span class="token boolean">NULL</span><span class="token punctuation">,</span>
	P_ID VARCHAR2<span class="token punctuation">(</span><span class="token number">2</span><span class="token punctuation">)</span>
<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> CHILD <span class="token keyword">ADD</span> <span class="token keyword">CONSTRAINT</span> C_PK <span class="token keyword">PRIMARY</span> <span class="token keyword">KEY</span><span class="token punctuation">(</span>C_ID<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> CHILD <span class="token keyword">ADD</span> <span class="token keyword">CONSTRAINT</span> C_FK <span class="token keyword">FOREIGN</span> <span class="token keyword">KEY</span><span class="token punctuation">(</span>P_ID<span class="token punctuation">)</span> <span class="token keyword">REFERENCES</span> PARENT <span class="token punctuation">(</span>P_ID<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>SQL 코드 참고 : <a href="https://www.youtube.com/watch?v=oyk3y1XzLW8">https://www.youtube.com/watch?v=oyk3y1XzLW8</a><br>
<strong>CASCADE</strong> 같은 옵션값을 줄수도 있음</p>
<p>참조 당하고 있는 부모 테이블의 인스턴스를 삭제할 경우,  자식에 인스턴스가 있기 때문에 지울 수 없다며 오류가 발생 =&gt; 해결방법은 아래와 같다.</p>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> CHILD <span class="token keyword">DROP</span> <span class="token keyword">CONSTRAINT</span> FK이름<span class="token punctuation">;</span> <span class="token comment">-- 제약조건 삭제 후</span>
<span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> CHILD <span class="token keyword">ADD</span> <span class="token keyword">CONSTRAINT</span> FK이름 <span class="token keyword">FOREIGN</span> <span class="token keyword">KEY</span> <span class="token punctuation">(</span>FK 컬럼<span class="token punctuation">)</span> <span class="token keyword">REFERENCES</span> PARENT <span class="token punctuation">(</span>부모의 PK 혹은 UK컬럼<span class="token punctuation">)</span> <span class="token keyword">ON</span> <span class="token keyword">DELETE</span> <span class="token keyword">CASCADE</span><span class="token punctuation">;</span>
</code></pre>
<p><strong>CASCADE</strong>로 인해 부모 테이블의 인스턴스 삭제 시, 자식에서 그 인스턴스를 참조하는 인스턴스들도 같이 삭제된다.</p>
<p><strong>CASCADE</strong> : <strong>PARENT</strong>삭제 시 <strong>CHILD</strong>해당 필드도 삭제<br>
<strong>SET NULL</strong> :  <strong>PARENT</strong>삭제 시 <strong>CHILD</strong>해당 필드 NULL로 UPDATE<br>
<strong>SET DEFAULT</strong>: <strong>PARENT</strong>삭제 시 <strong>CHILD</strong>해당 필드 DEFAULT 값으로 UPDATE<br>
<strong>RESTRICT</strong>: <strong>CHILD</strong>에 <strong>PK</strong> 값이 없는 경우만 <strong>PARENT</strong> 삭제<br>
<strong>NO ACTION</strong> : 참조 무결성 제약조건을 위배하는 행동 불가</p>
<h3 id="실무에서는-외래키-사용을-매우-지양하는-편-이걸-사용하면-하나의-부모-테이블을-참조하는-자식-테이블이-너무-많음-테이블-락이-걸릴-수도-있고-dml-과부하-가능성">실무에서는 외래키 사용을 매우 지양하는 편 이걸 사용하면 하나의 부모 테이블을 참조하는 자식 테이블이 너무 많음, 테이블 락이 걸릴 수도 있고, DML 과부하 가능성</h3>

