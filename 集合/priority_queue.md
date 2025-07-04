# 优先队列 (java.util.PriorityQueue)

```Java
offer(e) / add(e) //将元素插入队列。offer 在队列已满（对于有界队列）时返回 false，而 add 抛异常。PriorityQueue 是无界的，所以 add 总是成功。
poll() / remove() //检索并移除队列的头部（具有最高优先级的元素）。如果队列为空，poll 返回 null，remove 抛异常。
peek() / element() //检索但不移除队列的头部。如果队列为空，peek 返回 null，element 抛异常。
```

**示例1：（默认最小堆）**

```Java
import java.util.PriorityQueue;

public class PriorityQueueNaturalOrderExample {
    public static void main(String[] args) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(); // 默认最小堆

        minHeap.offer(50);
        minHeap.offer(20);
        minHeap.offer(80);
        minHeap.offer(10);

        System.out.println("PriorityQueue (min heap) elements (not necessarily sorted during iteration):");
        for(Integer num : minHeap) { // 迭代顺序不保证
            System.out.print(num + " ");
        }
        System.out.println();

        System.out.println("Polling elements (will be in priority order):");
        while (!minHeap.isEmpty()) {
            System.out.println("Peek: " + minHeap.peek() + ", Polled: " + minHeap.poll());
        }
        // 输出 (poll 的顺序):
        // Peek: 10, Polled: 10
        // Peek: 20, Polled: 20
        // Peek: 50, Polled: 50
        // Peek: 80, Polled: 80
    }
}
```

**示例2：（自定义排序）**
