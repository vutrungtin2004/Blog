---
title: "Java Collections Framework - Làm việc với List, Set, Map"
date: 2024-01-25T16:00:00+07:00
draft: false
description: "Hướng dẫn chi tiết về Java Collections Framework: List, Set, Map, Queue và các implementation phổ biến"
tags: ["Java", "Collections", "Data Structures", "ArrayList", "HashMap"]
categories: ["Java"]
author: "Trung Tín"
showAuthor: true
showDate: true
showDateUpdated: true
showReadingTime: true
showTableOfContents: true
showPagination: true
showTaxonomies: true
showWordCount: true
---

{{< lead >}}
📦 Master **Java Collections Framework** - bộ công cụ mạnh mẽ nhất để quản lý data structures! Từ ArrayList, HashMap đến TreeSet, bạn sẽ biết khi nào dùng cái gì!
{{< /lead >}}

## Giới thiệu Collections Framework

{{< alert "circle-info" >}}
**💡 Core Concept:** Collections Framework là nền tảng của **mọi ứng dụng Java** - hiểu rõ nó = viết code hiệu quả gấp 10 lần!
{{< /alert >}}

Java Collections Framework là một kiến trúc thống nhất để lưu trữ và thao tác với nhóm các objects. Framework bao gồm:
- **Interfaces**: Các kiểu dữ liệu trừu tượng
- **Implementations**: Các class cụ thể implement interfaces
- **Algorithms**: Các phương thức để thực hiện các thao tác (search, sort, ...)

## Hierarchy của Collections Framework

{{< mermaid >}}
graph TB
    A[Collection Interface] --> B[List]
    A --> C[Set]
    A --> D[Queue]
    
    B --> E[ArrayList]
    B --> F[LinkedList]
    B --> G[Vector]
    
    C --> H[HashSet]
    C --> I[LinkedHashSet]
    C --> J[TreeSet]
    
    D --> K[PriorityQueue]
    D --> F
    
    L[Map Interface] --> M[HashMap]
    L --> N[LinkedHashMap]
    L --> O[TreeMap]
    L --> P[Hashtable]
    
    style A fill:#ff6b6b
    style L fill:#4ecdc4
    style E fill:#95e1d3
    style M fill:#95e1d3
    
{{< /mermaid >}}

## 1. List Interface

{{< badge >}}
📝 Ordered Collection
{{< /badge >}}

List là một **ordered collection** (sequence) cho phép:
- ✅ Lưu trữ duplicate elements
- ✅ Truy cập elements theo index
- ✅ Insert elements ở bất kỳ vị trí nào

### ArrayList

{{< badge >}}
⚡ Most Popular
{{< /badge >}}

**ArrayList** là implementation phổ biến nhất của List, sử dụng dynamic array.

{{< alert "star" >}}
**⭐ Performance:** Random access **O(1)** - cực nhanh! Nhưng insert/delete ở giữa **O(n)** - cẩn thận!
{{< /alert >}}

#### Đặc điểm:
- {{< badge >}}O(1){{< /badge >}} Fast random access
- {{< badge >}}O(n){{< /badge >}} Slow insertion/deletion ở giữa
- {{< badge >}}Not Synchronized{{< /badge >}} Not thread-safe

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

public class ArrayListExample {
    public static void main(String[] args) {
        // Khởi tạo ArrayList
        List<String> fruits = new ArrayList<>();
        
        // Add elements
        fruits.add("Táo");
        fruits.add("Chuối");
        fruits.add("Cam");
        fruits.add("Dưa hấu");
        fruits.add("Cam"); // Duplicate allowed
        
        System.out.println("Danh sách trái cây: " + fruits);
        
        // Get element by index
        String firstFruit = fruits.get(0);
        System.out.println("Trái cây đầu tiên: " + firstFruit);
        
        // Update element
        fruits.set(1, "Nho");
        System.out.println("Sau khi update: " + fruits);
        
        // Remove element
        fruits.remove(0);           // Remove by index
        fruits.remove("Cam");       // Remove by object (first occurrence)
        System.out.println("Sau khi remove: " + fruits);
        
        // Size
        System.out.println("Số lượng: " + fruits.size());
        
        // Check contains
        boolean hasCam = fruits.contains("Cam");
        System.out.println("Có cam không? " + hasCam);
        
        // Iterate
        System.out.println("\n=== Duyệt qua ArrayList ===");
        
        // 1. For-each loop
        for (String fruit : fruits) {
            System.out.println("- " + fruit);
        }
        
        // 2. For loop with index
        for (int i = 0; i < fruits.size(); i++) {
            System.out.println(i + ": " + fruits.get(i));
        }
        
        // 3. Iterator
        var iterator = fruits.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
        
        // 4. forEach method (Java 8+)
        fruits.forEach(fruit -> System.out.println("=> " + fruit));
        
        // Sorting
        Collections.sort(fruits);
        System.out.println("Sau khi sort: " + fruits);
        
        // Clear all
        fruits.clear();
        System.out.println("Sau khi clear: " + fruits);
        System.out.println("Is empty? " + fruits.isEmpty());
    }
}
```

### LinkedList

**LinkedList** sử dụng doubly-linked list structure.

#### Đặc điểm:
- Fast insertion/deletion (O(1))
- Slow random access (O(n))
- Implements both List and Deque interfaces

```java
import java.util.LinkedList;

public class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<Integer> numbers = new LinkedList<>();
        
        // Add elements
        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        
        // Add at specific position
        numbers.addFirst(5);    // Add at beginning
        numbers.addLast(40);    // Add at end
        numbers.add(2, 15);     // Add at index 2
        
        System.out.println("LinkedList: " + numbers);
        
        // Get elements
        int first = numbers.getFirst();
        int last = numbers.getLast();
        System.out.println("First: " + first + ", Last: " + last);
        
        // Remove elements
        numbers.removeFirst();  // Remove first
        numbers.removeLast();   // Remove last
        System.out.println("After remove: " + numbers);
    }
}
```

## 2. Set Interface

{{< badge >}}
🎯 Unique Elements
{{< /badge >}}

Set là một collection **không cho phép duplicate elements**.

{{< alert "lightbulb" >}}
**💡 Use Case:** Khi cần đảm bảo **không có phần tử trùng lặp** - ví dụ: danh sách email, username, product IDs!
{{< /alert >}}

### HashSet

{{< badge >}}
⚡ O(1) Operations
{{< /badge >}}

**HashSet** sử dụng hash table để lưu trữ.

#### Đặc điểm:
- {{< badge >}}No Duplicates{{< /badge >}} Tự động loại bỏ duplicate
- {{< badge >}}Unordered{{< /badge >}} Không đảm bảo thứ tự
- {{< badge >}}O(1){{< /badge >}} Fast add/remove/contains
- {{< badge >}}Allows null{{< /badge >}} Cho phép 1 null value

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        Set<String> cities = new HashSet<>();
        
        // Add elements
        cities.add("Hà Nội");
        cities.add("TP.HCM");
        cities.add("Đà Nẵng");
        cities.add("Cần Thơ");
        cities.add("Hà Nội");  // Duplicate - will be ignored
        
        System.out.println("Cities: " + cities);
        System.out.println("Size: " + cities.size()); // 4, not 5
        
        // Check contains
        boolean hasHN = cities.contains("Hà Nội");
        System.out.println("Có Hà Nội? " + hasHN);
        
        // Remove
        cities.remove("Cần Thơ");
        System.out.println("After remove: " + cities);
        
        // Iterate
        for (String city : cities) {
            System.out.println("- " + city);
        }
    }
}
```

### TreeSet

{{< badge >}}
🔀 Auto-Sorted
{{< /badge >}}

**TreeSet** sử dụng Red-Black tree, **tự động sắp xếp** elements.

{{< alert "star" >}}
**⭐ Magic:** Elements được tự động sort! Không cần gọi `Collections.sort()` - TreeSet làm hết!
{{< /alert >}}

```java
import java.util.TreeSet;
import java.util.Set;

public class TreeSetExample {
    public static void main(String[] args) {
        Set<Integer> numbers = new TreeSet<>();
        
        // Add elements (unordered)
        numbers.add(50);
        numbers.add(10);
        numbers.add(30);
        numbers.add(20);
        numbers.add(40);
        
        // Automatically sorted
        System.out.println("TreeSet (sorted): " + numbers);
        // Output: [10, 20, 30, 40, 50]
        
        Set<String> names = new TreeSet<>();
        names.add("Nam");
        names.add("An");
        names.add("Bình");
        names.add("Cường");
        
        System.out.println("Names (sorted): " + names);
        // Output: [An, Bình, Cường, Nam]
    }
}
```

## 3. Map Interface

{{< badge >}}
🗺️ Key-Value Pairs
{{< /badge >}}

Map lưu trữ data dưới dạng **key-value pairs**.

{{< alert "circle-info" >}}
**💡 Core Concept:** Map **KHÔNG** extends Collection! Nó là interface riêng biệt cho key-value storage.
{{< /alert >}}

### HashMap

{{< badge >}}
🔥 Most Used
{{< /badge >}}

**HashMap** là implementation phổ biến nhất của Map.

{{< alert "star" >}}
**⭐ Best Performance:** HashMap cho performance tốt nhất với **O(1)** cho get/put operations!
{{< /alert >}}

#### Đặc điểm:
- {{< badge >}}Key-Value{{< /badge >}} Lưu theo cặp
- {{< badge >}}Unique Keys{{< /badge >}} Key không trùng
- {{< badge >}}O(1){{< /badge >}} Fast operations
- {{< badge >}}Unordered{{< /badge >}} Không sort
- {{< badge >}}1 null key{{< /badge >}} Multiple null values

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        // Khởi tạo HashMap
        Map<String, Integer> studentScores = new HashMap<>();
        
        // Put key-value pairs
        studentScores.put("An", 85);
        studentScores.put("Bình", 90);
        studentScores.put("Cường", 78);
        studentScores.put("Dũng", 92);
        
        System.out.println("Student Scores: " + studentScores);
        
        // Get value by key
        int scoreAn = studentScores.get("An");
        System.out.println("Điểm của An: " + scoreAn);
        
        // Update value
        studentScores.put("An", 88); // Update existing key
        System.out.println("Điểm An sau khi update: " + studentScores.get("An"));
        
        // Check key exists
        boolean hasBinh = studentScores.containsKey("Bình");
        System.out.println("Có Bình không? " + hasBinh);
        
        // Check value exists
        boolean hasScore100 = studentScores.containsValue(100);
        System.out.println("Có ai đạt 100 điểm? " + hasScore100);
        
        // Remove
        studentScores.remove("Cường");
        System.out.println("After remove: " + studentScores);
        
        // Get or default
        int scoreE = studentScores.getOrDefault("E", 0);
        System.out.println("Điểm của E (không tồn tại): " + scoreE);
        
        // Size
        System.out.println("Số lượng sinh viên: " + studentScores.size());
        
        // Iterate through Map
        System.out.println("\n=== Duyệt qua HashMap ===");
        
        // 1. Iterate through keys
        System.out.println("--- Keys ---");
        for (String name : studentScores.keySet()) {
            System.out.println(name);
        }
        
        // 2. Iterate through values
        System.out.println("--- Values ---");
        for (Integer score : studentScores.values()) {
            System.out.println(score);
        }
        
        // 3. Iterate through entries
        System.out.println("--- Entries ---");
        for (Map.Entry<String, Integer> entry : studentScores.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // 4. forEach (Java 8+)
        System.out.println("--- forEach ---");
        studentScores.forEach((name, score) -> {
            System.out.println(name + " => " + score);
        });
    }
}
```

### TreeMap

**TreeMap** tự động sắp xếp keys.

```java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        Map<String, String> capitals = new TreeMap<>();
        
        capitals.put("Vietnam", "Hà Nội");
        capitals.put("Japan", "Tokyo");
        capitals.put("USA", "Washington D.C.");
        capitals.put("France", "Paris");
        
        // Automatically sorted by key
        System.out.println("Capitals (sorted by country):");
        capitals.forEach((country, capital) -> {
            System.out.println(country + " -> " + capital);
        });
        // Output: France -> Paris
        //         Japan -> Tokyo
        //         USA -> Washington D.C.
        //         Vietnam -> Hà Nội
    }
}
```

## Ví dụ thực tế: Quản lý sinh viên

```java
import java.util.*;

class Student {
    private String id;
    private String name;
    private double gpa;
    
    public Student(String id, String name, double gpa) {
        this.id = id;
        this.name = name;
        this.gpa = gpa;
    }
    
    // Getters
    public String getId() { return id; }
    public String getName() { return name; }
    public double getGpa() { return gpa; }
    
    @Override
    public String toString() {
        return String.format("%s - %s (GPA: %.2f)", id, name, gpa);
    }
}

public class StudentManagement {
    public static void main(String[] args) {
        // List để lưu danh sách sinh viên
        List<Student> students = new ArrayList<>();
        students.add(new Student("SV001", "Nguyễn Văn An", 3.5));
        students.add(new Student("SV002", "Trần Thị Bình", 3.8));
        students.add(new Student("SV003", "Lê Văn Cường", 3.2));
        students.add(new Student("SV004", "Phạm Thị Dung", 3.9));
        
        // Map để tra cứu nhanh sinh viên theo ID
        Map<String, Student> studentMap = new HashMap<>();
        for (Student student : students) {
            studentMap.put(student.getId(), student);
        }
        
        // Set để lưu danh sách môn học (không trùng)
        Set<String> subjects = new HashSet<>();
        subjects.add("Java Programming");
        subjects.add("Database");
        subjects.add("Web Development");
        subjects.add("Data Structures");
        
        // Hiển thị thông tin
        System.out.println("=== DANH SÁCH SINH VIÊN ===");
        students.forEach(System.out::println);
        
        System.out.println("\n=== TRA CỨU SINH VIÊN ===");
        Student sv = studentMap.get("SV002");
        System.out.println("Tìm SV002: " + sv);
        
        System.out.println("\n=== DANH SÁCH MÔN HỌC ===");
        subjects.forEach(subject -> System.out.println("- " + subject));
        
        // Sắp xếp sinh viên theo GPA
        System.out.println("\n=== SINH VIÊN THEO GPA (GIẢM DẦN) ===");
        students.sort((s1, s2) -> Double.compare(s2.getGpa(), s1.getGpa()));
        students.forEach(System.out::println);
    }
}
```

## Chọn Collection phù hợp

| Cần | Chọn |
|-----|------|
| Ordered, allow duplicates | **ArrayList** |
| Fast insertion/deletion | **LinkedList** |
| Unique elements, unordered | **HashSet** |
| Unique elements, sorted | **TreeSet** |
| Key-value pairs | **HashMap** |
| Key-value pairs, sorted | **TreeMap** |

## Kết luận

{{< timeline >}}

{{< timelineItem icon="list" header="List Interface" badge="Ordered" >}}
**ArrayList** cho random access nhanh, **LinkedList** cho insert/delete nhanh. Chọn theo use case!
{{< /timelineItem >}}

{{< timelineItem icon="circle-check" header="Set Interface" badge="Unique" >}}
**HashSet** cho performance tốt nhất, **TreeSet** khi cần auto-sort. Never worry về duplicates!
{{< /timelineItem >}}

{{< timelineItem icon="map" header="Map Interface" badge="Key-Value" >}}
**HashMap** là king of performance, **TreeMap** khi cần sorted keys. Master Map = master Java!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ Best Practice:** 
- Luôn dùng **interface** khi khai báo: `List<String> list = new ArrayList<>()`
- Chọn **ArrayList** as default, optimize sau nếu cần
- **HashMap** cho hầu hết key-value use cases
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html" target="_blank" >}}
📚 Java Collections Overview
{{< /button >}}

{{< button href="https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html" target="_blank" >}}
📖 API Documentation
{{< /button >}}

{{< button href="https://www.oreilly.com/library/view/effective-java/9780134686097/" target="_blank" >}}
📕 Effective Java
{{< /button >}}

