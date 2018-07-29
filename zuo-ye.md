```
import java.util.Scanner;
import java.util.regex.Pattern;

public class Main {
    final static char[] OUT = { 'W', 'R' };

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            int m = scanner.nextInt(), n = scanner.nextInt();
            scanner.nextLine();
            String[] links = scanner.nextLine().split(";");
            int[][] val = new int[2 * n + 1][2 * m + 1];
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    val[2 * i + 1][2 * j + 1] = 1;
                }
            }
            String pattern = "[0-9]+(,)[0-9]+( )[0-9]+(,)[0-9]";
            for (String link : links) {
                if (!Pattern.matches(pattern, link)) {
                    System.out.println("Incorrect command format.");
                    return;
                }
                int index1, index2, index3;
                index1 = link.indexOf(',');
                int x1 = Integer.parseInt(link.substring(0, index1));
                index2 = link.indexOf(' ');
                int y1 = Integer.parseInt(link.substring(index1 + 1, index2));
                index3 = link.indexOf(',', index2);
                int x2 = Integer.parseInt(link.substring(index2 + 1, index3));
                int y2 = Integer.parseInt(link.substring(index3 + 1));
                if (x1 < 0 || x1 >= n || y1 < 0 || y1 >= m || x2 < 0 || x2 >= n || y2 < 0 || y2 >= m) {
                    System.out.println("Number out of range.");
                    return;
                } else if (x1 == x2 && Math.abs(y1 - y2) == 1 || y1 == y2 && Math.abs(x1 - x2) == 1) {
                    val[x1 + x2 + 1][y1 + y2 + 1] = 1;
                } else {
                    System.out.println("Incorrect command format.");
                    return;
                }
            }
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 2 * n + 1; i++) {
                for (int j = 0; j < 2 * m; j++) {
                    sb.append("[" + OUT[val[i][j]] + "] ");
                }
                sb.append("[" + OUT[val[i][2 * m]] + "]\n");
            }
            System.out.println(sb.toString());
        }
    }
}
```

---

以上为程序源代码

