# Dockerize

- 로컬에서 개발하던 환경을 앞으로 지급 받을 AWS 서버 환경에 직접 배포하게 되면 생소한 환경적인 문제로 배포 환경에서 예상처럼 동작하길 기대하기 어려울 수 있는데 이를 해결하기 위함

## 장점

- os를 포함한 설치 과정이 Dockerfile로 문서화, 수정 이력은 버전 관리가 되어 롤백하기 편함
- 개발과정에서 필요한 환경 구성이 편리해짐
- os를 가리지 않고 동일하게 동작함을 보장하는 불변서버로 제작가능

[참고](https://hub.docker.com/search)  
[도커 튜토리얼](https://www.44bits.io/ko/post/easy-deploy-with/docker)

---

1. 설치 및 명령어 실습

```
개발환경 설정
설치 후
docker -v

docker run hello-world

--
컨테이너 조회
PS C:\Users\seong> docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
7d7a552e8c26   hello-world   "/hello"   5 seconds ago   Exited (0) 4 seconds ago             mystifying_bouman

ex)Hello wolrd 컨테이너 삭제
docker rm [컨테이너 ID 또는 name]

도커 이미지 조회
PS C:\Users\seong> docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   3 months ago   13.3kB

도커 이미지 삭제ex)
docker rmi hello-world
```

---

2. 여러가지 서비스 도커로 이용해 보기

```
docker run --name myjenkins -d -p 9080:8080 jenkins/jenkins

-d: 백그라운드 데몬으로 실행
-p: 가상머신에서 동작하는 서비스에 docker proxy 서비스가 NAT기능을 수행해 포트 포워딩을 해주어 실제 머신 IP로 서비스에 접속이 가능하도록 해준다.

jenkins서버 컨테이너의 bash 실행 후 컨테이너의 os버전 확인
PS C:\Users\seong> docker exec -it myjenkins bash
jenkins@b58359ae1cbb:/$ cat /etc/issue
Debian GNU/Linux 10 \n \l

-it옵션은
-i(interactive) 표준 입출력을 키보드와 화면을 통해 가능하도록 해주는 옵션
-t는 텍스트 기반 터미널을 에뮬레이션 해주는 옵션

jenkins@b58359ae1cbb:/$ cat /var/jenkins_home/secrets/initialAdminPassword
admin패스워드 저장된 파일 확인



```

---

3. 관통 프로젝트 코드로 도커 이미지 제작
