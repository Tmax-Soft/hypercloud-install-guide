
# Federation 설치 가이드

## 구성 요소 및 버전
* federation verion: v0.3.0 구축 요망

## Prerequisites
* helm version 2

## 폐쇄망 설치 가이드
## Step 0. 환경변수 설정 및 폐쇄망 작업
* 목적 : Fed 구축에 필요한 환경 변수 설정
* 생성 순서 :
    * script 실행 권한 부여
    ```bash
    $ chmod +x *.sh
    ```
    * 환경 변수를 적절히 수정 및 적용
    ```bash
    $ vim 0.setEnv.sh
    $ source ./0.setEnv.sh
    ```
    * 폐쇄망 작업 수행
    ```bash
    $ ./1.setPrivate.sh
    ```
## Step 1. federation 배포
* 목적 : federation 배포
* 배포 순서 :
    * federation 배포 script 수행
    ```bash
    $ ./2.deployFed.sh
    ```
    * 정상 동작 확인
    ```bash
    $ kubectl get pod -n kube-federation-system (step.0에서 설정한 namespace)
    ```
    ![image](figure/pod.JPG)
## Step 2. federation 제거
* 목적 : federation 제거
* 제거 순서 :
    * federation 제거 script 수행
    ```bash
    $ ./3.deleteFed.sh
    ```
## Public Install Steps
0. [Binary 설치](https://github.com/tmax-cloud/hypercloud-install-guide/tree/master/Fed#step-0-binary-%EC%84%A4%EC%B9%98)
1. [helm을 통한 federation 설치](https://github.com/tmax-cloud/hypercloud-install-guide/tree/master/Fed#step-1-helm%EC%9D%84-%ED%86%B5%ED%95%9C-federation-%EC%84%A4%EC%B9%98)

## Step 0. Binary 설치
* 목적 : Fed 구축에 필요한 binary 설치
* 생성 순서 : 
    * 환경변수 설정
      ```bash
      $ export VERSION=0.1.0-rc5
      $ export OS=linux
      $ export ARCH=amd64
      ```
    * kubefedctl 설치
      ```bash
      $ curl -LO https://github.com/kubernetes-sigs/kubefed/releases/download/v${VERSION}/kubefedctl-${VERSION}-${OS}-${ARCH}.tgz
      $ tar -zxvf kubefedctl-*.tgz
      $ chmod u+x kubefedctl
      $ sudo mv kubefedctl /usr/local/bin/
      ```

## Step 1. helm을 통한 federation 설치
* 목적 : federation 구축
* 생성 순서 : 
    * KubeFed 차트 저장소를 로컬 저장소에 추가
      ```bash
      $ helm repo add kubefed-charts https://raw.githubusercontent.com/kubernetes-sigs/kubefed/master/charts
      $ helm repo list
      ```
      ![image](figure/helm_repo_list.JPG)
    * 지정된 버전으로 kubefed chart 설치
      ```bash
      $ helm search kubefed --devel
      ```
      ![image](figure/helm_search.JPG)
      ```bash
      $ helm install kubefed-charts/kubefed --name kubefed --version=<x.x.x> --namespace kube-federation-system --devel
      ```
    * 정상 동작 확인
      ```bash
      $ kubectl get pod -n kube-federation-system
      ```
      ![image](figure/pod.JPG)
