---
title: Batch
summary: spring batch
categories: [SpringBoot]
comments: true
---
sssss

### Spring Batch
대용량 데이터를 처리하기 위해서 Spring에서 지원하는 일괄처리 기능이다.
스케쥴러와는 전혀 다른데 차이점은 스케쥴러의 경우 중간에 에러가 발생하여 끊길 경우 처음부터 진횅되어야하는 경우가 생긴다.
batch는 이러한 상황에서 중간에 끊겨도 이어서 진행이 가능하며 설정에 따라 사용자가 조정할 수도 있으며 일단 가볍고 빠르기 때문에 대용량을 처리하는데 적합하다.

### batch 구성도
1. 배치의 최소 실행 단위를 jab 이라고 한다.
2. 하나의 Job은 하나 이상의 Step을 가진다.
3. 하나의 Step은 다음과 같은 작업을 가질 수 있다. : [read, process, write] == chunk
    * read, process, write 세 개의 실행 단위를 하나로 묶은 것을 'chunk'라고 칭한다.

<pre>
JobLauncher --> Job --> Step --> read
                             --> process
                             --> write

JobRepository --> JobLauncher
              --> Job
              --> Step
</pre>

### Job Annotaion
**@EnableBatchProcessing** : 배치 기능을 활성화 시켜준다.
```
>> @EnableBatchProcessing 제공하는 클래스
1. JobRepository : 실행중인 잡의 상태를 기록하는데 사용
2. JobLauncher : 잡을 구동하는데 사용
3. JobExplorer : JobRepository를 사용해 읽기 전용 작업을 수행하는데 사용
4. JobRegistry : 특정한 런처 구현체를 사용할때 잡을 찾는 용도로 사용
5. PlaformTransactionManager : 잡 진행 과정에서 트랜잭션을 다루는데 사용
6. JobBuilderFactory : 잡을 생성하는 빌더
7. StepBuilderFactory : 스텝을 생성하는 빌더
```

@JobScope : Step에 parameter를 부여할 수 있도록 한다. (@Value와 함께 사용)<br/>
@Value : Step의 parameter를 가져온다.

```
>> @Value 사용 예제
@Bean
@JobScope
public Step read(@Value("#{jobParameters[requestDate]}") String requestDate) {...}
```

### Batch 특성
* batch는 특정 테이블들이 있어야 동작하며 해당 테이블들은 설정을 통해 자동으로 생성이 가능하다.
* 하나의 Job은 실행이 되면 BATCH_JOB_INSTANCE 테이블에 남는다.
* 이미 실행된 job은 다시 실행 되지 않으며 파라미터가 다른 경우에는 실행이 된다. 같은 파라미터를 가진 job을 실행 시키려면 값을 다르게 주어야 한다.
* (동일한 Job Parameter는 여러개 존재할 수 없다)
* Job의 성공/실패 내역을 BATCH_JOB_EXECUTION 테이블이 가진다.
* job은 동일한 파라미터의 성공을 중복으로 기록하지 않으며 실패가 기록 시 동일한 파라미터의 성공은 기록한다.

### Batch Status & Exit Status
- *batch는 결과에 따라 다른 반응을 보여줄 수 있다.*
```
@Bean
public Job job() {
    return jobBuilderFactory.get("job")
            .start(read())
                .on("FAILED") // FAILED일 경우
                .to(write()) // write 이동
                .on("*") // write 결과와 상관없이
                .end() // flow 종료
            .from(read()) // read 실행 시
                .on("*") // FAILED 아닌 경우
                .to(process()) // process 이동
                .next(write()) // process 정상 종료 시 write 이동
                .on("*") // write 결과와 상관없이
                .end() // flow 종료
            .end() // job 종료
            .build();
}
```
* .on()
  * 캐치할 ExitStatus 지정
  * *일 경우 모든 ExitStatus가 지정된다.
* .to()
  * 다음으로 이동할 Step 지정



....writing
