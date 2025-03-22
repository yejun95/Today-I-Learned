## reduce + for in, for of 이중배열을 통해 특정 값 뽑아내기
- 데이터 정의 순서

1. 공통 key로 뽑아낼 값을 정의 (= `bus_year_month`)
2. `reduce`함수를 통해 해당 속성을 key값으로 배열을 만든다.
3. 년-월이 같은 값 끼리 배열객체로 변환
4. `for in`을 통해 같은 key값끼리 변수에 담는다.
5. `for of`를 통해 4번 항목에서 담긴 변수를 출력한다. (key가 같은 객체끼리 묶인다.)
<br>
<hr>
<br>

✔ **1. 공통 키 정의**
- 아래 기초데이터에서 `bus_year_month`를 key로 정의
<br>
<br>

✔ **2. reduce 사용**
- `reduce`를 이용해 공통 key 별로 새 배열 생성
```javascript
const busYearData = 기초데이터변수.reduce((acc, current) => {
      const yearMonth = current.bus_year_month;
      if(!acc[yearMonth]) {
        acc[yearMonth] = [];
      };
      acc[yearMonth].push(current);
      return acc;
    }, {});
```
<br>
<br>

✔ **3. key를 가진 배열로 변환**
- reduce를 사용하여 공통 key값으로 배열을 만들면 기초데이터가 아래와 같이 변경된다.

- `bus_year_month`가 같은 것끼리 key값을 가진 새 배열이 생성된다.

- 데이터가 많아 일부 생략하였음.
```javascript
{
  '2023-08': [
    RowDataPacket {
      bus_title: 'TC_CONTRACT_1010_신규사업추가',
      contractId: '08800fb047e411ee8c2f2bb8e316857c-S1',
      bus_year_month: '2023-08',
    },
    RowDataPacket {
      bus_title: '테스트 사업',
      contractId: 'e5383f80d1e811edb080a3e7df28f270-S1',
      bus_year_month: '2023-08',
    }
  ],
  '2023-04': [
    RowDataPacket {
      bus_title: '자기승인테스트',
      contractId: '0ca912c0df5311edac797d6dd8e4406d-S1',
      bus_year_month: '2023-04',
    },
    RowDataPacket {
      bus_title: '신규사업추가 후 승인요청',
      contractId: '3aed23c0ddcc11edbb2d85e64f992d41-S1',
      bus_year_month: '2023-04',
    }
  ],
  '2023-05': [
    RowDataPacket {
      bus_title: '사업년월 테스트',
      contractId: '09931700d83e11eda00d81f143920984-S2',
      bus_year_month: '2023-05',
    },
    RowDataPacket {
      bus_title: '사업년월 테스트',
      contractId: '09931700d83e11eda00d81f143920984-S2',
      bus_year_month: '2023-05',
    }
  ],
  '2023-03': [
    RowDataPacket {
      bus_title: 'test9',
      contractId: '8333d490cd3b11ed910ef99608001c76-S1',
      bus_year_month: '2023-03',
    },
    RowDataPacket {
      bus_title: '인테크 모니터링 시스템 점검 출장',
      contractId: '3c61d8b0d42411ed85d0930a87568219-S1',
      bus_year_month: '2023-03',
    }
  ]
}
  ```
<br>

- 이제 해당 데이터를 for문을 사용해 가공하여 사용할 수 있도록 한다.
<br>
<br>

✔ **4. for in : 같은 key 끼리 변수에 담는다.**

```javascript
for (const key in groupData) {
      const yearMonth = groupData[key]
};

```
<br>

- `for in`을 사용하여 객체를 순회하고 key값을 뽑을 수 있다.

- `console.log`로 `key`와 yearMonth`를 찍어보자.

```javascript
console.log (key)
///////////////////////////

console.log test/160.dash2.test.js:107
    2023-08

  console.log test/160.dash2.test.js:107
    2023-04

  console.log test/160.dash2.test.js:107
    2023-05

  console.log test/160.dash2.test.js:107
    2023-03
```
- `for in`특징으로 인해 데이터의 key요소만 가져온다.

- 이후 `groupData[key]`를 통해 반복문안에서 변수에 넣어준다.

- 데이터가 많아 일부만 적음

```javscript
console.log(yearMonth)
//////////////////////////////////

console.log test/160.dash2.test.js:107
    [
      RowDataPacket {
        bus_title: 'test9',
        contractId: '8333d490cd3b11ed910ef99608001c76-S1',
        bus_year_month: '2023-03'
    },
       RowDataPacket {
        bus_title: 'test',
        contractId: 'dd951420cd3c11ed957133d349b51321-S2',
        bus_year_month: '2023-03',
    }
......

console.log test/160.dash2.test.js:107
    [
      RowDataPacket {
        bus_title: '사업년월 테스트',
        contractId: '09931700d83e11eda00d81f143920984-S2',
        bus_year_month: '2023-05',
    },
      RowDataPacket {
        bus_title: '사업년월 테스트',
        contractId: '09931700d83e11eda00d81f143920984-S2',
        bus_year_month: '2023-05',
    }
.......  

console.log test/160.dash2.test.js:107
    [
      RowDataPacket {
        bus_title: '자기승인테스트',
        contractId: '0ca912c0df5311edac797d6dd8e4406d-S1',
        bus_year_month: '2023-04',
.....
```
<br>

- key로 정의해놓은 같은 `bus_year_month`끼리 배열 목록이 된 것을 확인할 수 있다.

- 이후 `for of`를 활용해 배열을 순회해서 원하는 값을 뽑을 수 있다.
<br>
<br>

✔ **5. for of**
- 해당 형식으로 진행된 이유는 jest를 활용한 월별데이터를 뽑아내서 검증하기 위해서였다.

- 그로 인해 마지막 `for of`문은 각 데이터의 `bus_year_month`가 뽑아낸 key값이랑 같은지 확인하였다.

- 이렇게 되면 각 월에 해당하는 데이터가 맞는지 검증할 수 있다.

```javascript
for (const key in groupData) {
      const yearMonth = groupData[key]
      for (const item of yearMonth) {
        expect(item.bus_year_month).toBe(key);
      }
    };
```
<br>
<hr>
<br>

**해당 코드가 사용된 테스트 케이스**
```javascript
test("TC_DASH2_1020_월별세금계산서현황", async () => {
    req.query = yearData;
    
    await controllerDashboard.selectDashSalesInfo(req, res);
    // 2023년도의 세금계산서 데이터를 가져온다.
    const data_2023 = res._getData().data.chartData.filter(
      item => item.bus_year_month.startsWith('2023'));

    // bus_year_month 값을 기준으로 월별로 나눈다.
    const groupData = data_2023.reduce((acc, current) => {
      const yearMonth = current.bus_year_month;
      if(!acc[yearMonth]) {
        acc[yearMonth] = [];
      };
      acc[yearMonth].push(current);
      return acc;
    }, {});
    for (const key in groupData) {
      const yearMonth = groupData[key]
      for (const item of yearMonth) {
        expect(item.bus_year_month).toBe(key);
      }
    };
    expect(res.statusCode).toBe(201);
    expect(res._getData().code).toBe('SUCC');
  });
```
<br>
<br>
<br>
<br>
<hr>

## 기초데이터
```
[
  RowDataPacket {
    bus_title: 'TC_CONTRACT_1010_신규사업추가',
    contractId: '08800fb047e411ee8c2f2bb8e316857c-S1',
    bus_year_month: '2023-08',
  },
  RowDataPacket {
    bus_title: '테스트 사업',
    contractId: 'e5383f80d1e811edb080a3e7df28f270-S1',
    bus_year_month: '2023-08',
  },
  RowDataPacket {
    bus_title: 'TC_CONTRACT_1010_신규사업추가',
    contractId: 'a5087fd047e311eea72b87c8f0294b37-S1',
    bus_year_month: '2023-08',
  },
  RowDataPacket {
    bus_title: 'TC_CONTRACT_1010_신규사업추가',
    contractId: '846ac75047e411eeaa405dc3d230d859-S1',
    bus_year_month: '2023-08',
  },
  RowDataPacket {
    bus_title: 'test',
    contractId: '537adae047e411ee8ea575d6086b184c-S1',
    bus_year_month: '2023-08',
  },
  RowDataPacket {
    bus_title: '자기승인테스트',
    contractId: '0ca912c0df5311edac797d6dd8e4406d-S1',
    bus_year_month: '2023-04',
  },
  RowDataPacket {
    bus_title: '사업년월 테스트',
    contractId: '09931700d83e11eda00d81f143920984-S2',
    bus_year_month: '2023-05',
  },
  RowDataPacket {
    bus_title: '사업년월 테스트',
    contractId: '09931700d83e11eda00d81f143920984-S2',
    bus_year_month: '2023-05',
  },
  RowDataPacket {
    bus_title: '사업년월 테스트',
    contractId: '09931700d83e11eda00d81f143920984-S2',
    bus_year_month: '2023-05',
  },
  RowDataPacket {
    bus_title: '신규사업추가 후 승인요청',
    contractId: '3aed23c0ddcc11edbb2d85e64f992d41-S1',
    bus_year_month: '2023-04',
  },
  RowDataPacket {
    bus_title: 'test9',
    contractId: '8333d490cd3b11ed910ef99608001c76-S1',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test9',
    contractId: '8333d490cd3b11ed910ef99608001c76-S2',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test10',
    contractId: '5c7b9c10cd3c11ed8c8313b9ed465943-S2',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: '인테크 모니터링 시스템 점검 출장',
    contractId: '3c61d8b0d42411ed85d0930a87568219-S1',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: '매출 테스트',
    contractId: 'b0f8f810d8da11ed8373afffd6eafc48-S1',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test',
    contractId: 'dd951420cd3c11ed957133d349b51321-S1',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test',
    contractId: 'dd951420cd3c11ed957133d349b51321-S1',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test',
    contractId: 'dd951420cd3c11ed957133d349b51321-S1',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test',
    contractId: 'dd951420cd3c11ed957133d349b51321-S2',
    bus_year_month: '2023-03',
  },
  RowDataPacket {
    bus_title: 'test10',
    contractId: '5c7b9c10cd3c11ed8c8313b9ed465943-S1',
    bus_year_month: '2023-03',
  }
]
```
