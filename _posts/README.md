Closest pair of points 코드

=========================

##분할정복

<pre><code>{
    public int closestPair(ArrayList<Point> p, int start, int end){


        int i = end-start + 1;
    
        if(i <= 3) return bruteForce(p, start, end);
        int num = (start+end)/2;
    
        int CPL = closestPair(p, start, num);
        int CPR = closestPair(p, num+1, end);
        int CPC = (int)((p.get(num+1).x - p.get(num).x)*(p.get(num+1).x - p.get(num).x))
                + (int)((p.get(num+1).y - p.get(num).y)*(p.get(num+1).y - p.get(num).y));
        if(CPC< 0) CPC = -CPC;
        int d = Math.min(CPC, Math.min(CPL, CPR));
    
        return d;
    
    }
    
​	분할정복 부분에서는 배열의 중심 부분을 기준으로 절반씩 나눠가면서 분할을 진행했습니다. 메소드에서 시작과 끝 인덱스를 받아서 배열의 크기를 계산한 다음, 크기가 3이하이면 다른 메소드에서 점들 사이의 거리를 구해 가장 작은 값을 반환했습니다. 여기서 주의할 점은 분할의 기준이되는 중간 값들의 거리도 비교해 주어야 한다는 것입니다.  중간 인덱스를 기준으로 왼쪽과 오른쪽으로 분할하다보면 중간인덱스들 사이의 거리를 계산하지 않게 됩니다. 하지만 중간값들 사이의 거리가 최소가 될 수 있기 때문에 반드시 두점 사이의 거리를 구해 왼쪽에서 반환된 값과 오른쪽에서 반환된 값을 비교해 주어야 합니다.

##BRUTEFORCE

<pre><code>{

```
public int bruteForce(ArrayList<Point> p, int start, int end) {
    int min = 214700000;

    for(int i = start; i < end; i++){
        for(int j =i + 1; j <= end; j++) {
            int cmp = (int)((p.get(j).x - p.get(i).x)*(p.get(j).x - p.get(i).x))
                    + (int)((p.get(j).y - p.get(i).y)*(p.get(j).y - p.get(i).y));
            if(cmp < 0) cmp = -cmp;
            if(cmp < min) min = cmp;
        }
    }
```

}</code></pre>

​	bruteforce 부분에서는 int의 최대값이 21억 4천7만 정도임을 이용해 최소값의 초기값을 설정하고 , 그 이후에 배열의 시작과 끝까지 각 점들의 거리를 구해 최소값과 비교해서 마지막에 가장 작은 값을 반환해 줍니다. 여기서 배열의 크기는 항상 3이하이기 때문에 시각 복잡도는 항상 O(상수)가 됩니다. 따라서 bruteforce의 시간복잡도는 O(1)입니다. 

##main, Point class

<pre><code>
class Point implements Comparable<Point>{
    int x, y;
    Point(int x, int y){
        this.x =x;
        this.y = y;
    }
    public int compareTo(Point p){
        return x - p.x;
    }

}
</code></pre>

<pre><code>
    public static void main(String[] args) {
        long seed = System.currentTimeMillis();
        Random rand = new Random(seed);


        ArrayList<Point> p = new ArrayList<Point>();
    
        for(int i =0; i < 5; i++) {
            int x = rand.nextInt(10) + 1;
            int y = rand.nextInt(10) + 1;
    
            p.add(new Point(x, y));
        }
        Collections.sort(p);
        for(Point s : p) {
            System.out.printf("%d %d", s.x, s.y);
            System.out.println();
        }
        Closest point = new Closest();
    
        System.out.println(Math.sqrt(point.closestPair(p, 0, p.size()-1)));
    }
</code></pre>

​	main함수에서는 x와 y의 값을 입력받기 위해 Point class를 만들어 객체의 형태로 값을 저장할 수 있도록 하였고, 그 이후 랜덤으로 x와 y값을 입력받아 ArrayList에 각각 저장하고 x값을 기준으로 정렬하였습니다. x값으로 정렬을 하면 좌표처럼 값을 비교할 수 있게 됩니다. 그리고 앞에서 설명한 분할 정복 과정을 수행하기 위해 Closest객체를 만들어 closestPair메소드를 호출하였습니다.

##결론, 느낀점

​	closest pair of points문제를 해결하면서 느낀점은 알고리즘에서 시간 복잡도의 중요성이었습니다.  사실 closest pair of points 문제는 브르투포스 알고리즘 만으로 문제를 해결할 수 있습니다. 하지만 모든 점을 한번씩 다 비교해야하기 때문에 O(n^2)의 시간 복잡도로 문제를 해결하게 됩니다. 이 방식대로 라면, n의 크기가 커질수록 알고리즘은 비효율적입니다. 하지만 합병정렬의 원리를 기반으로 분할정복하여 문제를 해결해보면, 분할하는 과정에서 logn, 합병하는 과정 n, 그리고 브루투포스 에서 0(1)의 시간 복잡도를 가져, 총 시간 복잡도는O(nlogn)+O(1)됩니다. 결론적으로 O(nlogn)의 시간복잡도를 가지기 때문에 O(n^2)보다 훨씬 효율적인 알고리즘으로 문제를 해결할 수 있습니다.