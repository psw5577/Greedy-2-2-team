## 과제: 부분 배낭 문제
## 1. 부분 배낭 문제란?
부분 배낭 문제는 n개의 물건이 있고, 각 물건은 무게와 가치를 가지고 있으며, 배낭이 한정된 무게의 물건들을 담을 수 있을 때, 최대의 가치를 갖도록 배낭에 넣을 물건들을 정하는 문제이다.

물건을 부분적으로 배낭에 담을 수 있으므로, 최적값을 위해서 **욕심을 내어** 단위 무게 당 가장 값나가는 물건을 배낭에 넣고, 계속해서 그 다음으로 값나가는 물건을 넣는다.

그런데 만일 그 다음으로 값나가는 물건을 **통째로** 배낭에 넣을 수 없게 되면, 배낭에 넣을 수 있을 만큼만 물건을 부분적으로 배낭에 담는다.

## 2.알고리즘 설계 과정
1. 각 물건에 대해 무게 당 가치를 계산한다
2. 물건 리스트 S에 무게 당 가치를 기준으로 내림차순으로 정렬한다.
3. 배낭에 담은 물건리스트 L과 배낭에 담긴 물건의 무게 W, 배낭의 담긴 가치 V를 0으로 초기화한다.
4. S에서 현재 가장 무게 당 가치가 큰 물건 X를 가져온다.

5. (W + X의 무게)가 배낭의 용량 C보다 작거나 같을 동안 반복
* 물건 X를 L에 추가한다
* W = W + X의 무게
* V = V + X의 가치
* X를 S에서 제거
* S에서 현재 가장 무게 당 가치가 큰 물건 X를 가져온다

6. 만일 C - W 가 0이 아니면, 즉 배낭에 빈 공간이 아직 있다면
* 물건 X를 C - W 만큼만 L에 추가한다.
* V = V + C - W 만큼의 X의 가치

7. 리스트 L과 가치 V를 출력한다.

## 3. 자바 코드

``` java
import java.util.*;

public class Fractional {
    private double Fractional_Knapsack (int[][] S, double[][] L, int C) { // (물건이 담긴 리스트, 배낭에 담긴 물건리스트, 배낭의 용량)
        int W = 0; // 배낭에 담긴 물건의 무게
        double V = 0; // 배낭의 가치
        int[] X = {0, 0}; // 물건 X, X[0]: 무게, X[1]: 가치
        int t = 0;

        X[0] = S[0][0];
        X[1] = S[0][1];
        while (C >= (W + X[0])) { // 현재 배낭에 담긴 물건의 무게와 담을 물건의 무게의 합이 배낭의 용량보다 적으면
            L[t][0] = X[0];
            L[t][1] = X[1]; // 리스트에 물건 X를 추가

            W += X[0]; // 물건 X의 무게 추가
            V += X[1]; // 물건 X의 가치 추가

            t++;
            X[0] = S[t][0]; // 다음으로 무게 당 가치가 큰 물건을 꺼내옴
            X[1] = S[t][1];
        }

        if ((C-W) != 0) { // 배낭에 빈공간이 남아 있으면
            double a = (double) (C-W)/X[0]; // 남은 무게만큼 물건의 값을 계산할 상수
            L[t][0] = X[0] * a;
            L[t][1] = X[1] * a;

            V += (X[1] * a);
        }
        return V;
    }

    public void sorting(int[][] S) {
        double[] tp = new double[S.length]; // 물건의 무게 당 가치를 담을 배열
        for (int i = 0; i<S.length; i++)
            tp[i] = (double)S[i][1]/S[i][0];  // 무게 당 가치 = 가치/무게

        for (int i = 0; i<S.length; i++) {
            for (int j = i+1; j<S.length; j++) {
                if (tp[i] < tp[j]) {
                    double tmp = tp[i];
                    tp[i] = tp[j];
                    tp[j] = tmp;

                    int temp = S[i][0];
                    S[i][0] = S[j][0];
                    S[j][0] = temp;

                    temp = S[i][1];
                    S[i][1] = S[j][1];
                    S[j][1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("배낭의 용량을 정수로 입력하세요:");
        int C = sc.nextInt();

        System.out.print("물건의 개수를 입력하세요:");
        int n = sc.nextInt();
        int[][] S = new int[n][2]; // 입력받은 수 만큼 물건을 담을 배열 생성

        System.out.println("물건의 무게와 가치를 정수로 입력하세요 (그램 원)");
        for (int i = 0; i < n; i++) {
            S[i][0] = sc.nextInt(); // 0열 : 무게
            S[i][1] = sc.nextInt(); // 1열 : 가치
        }

        double[][] L = new double[n][2]; // 배낭에 담긴 물건의 리스트

        Fractional Fr = new Fractional();
        Fr.sorting(S); // 물건 리스트 S를 무게당 가치를 기준으로 내림차순 정

        double V = Fr.Fractional_Knapsack(S, L, C);

        System.out.println("배낭의 담긴 물건:");
        for (int i = 0; i< L.length; i++) {
            if (L[i][0] == 0) break;
            System.out.println(L[i][0] + " " + L[i][1]);
        }

        System.out.println("배낭의 가치는 " + V);
    }
}
```
