+++ 
draft = true
date = 2024-01-31T07:17:05+03:00
title = "SOLID"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

- **S**ingle Responsibility Principle (SRP)
- **O**pen/Closed Principle (OCP)
- **L**iskov Substitution Principle (LSP)
- **I**nterface Segregation Principle (ISP)
- **D**ependency Inversion Principle (DIP)

## 1. Принцип единственной ответственности (SRP)

Класс должен иметь только одну причину для изменений. Ответственность - причина для изменения.

### Преимущества

- Код становится меньше, чище, читабельнее.
- Не надо затрагивать (разбирать) логику, не связанную с задачей.
- Не надо тестировать лишний код.

### Task

Необходимо хранить и выводить информации о `Employee`, определять квалификацию по опыту, генерировать уникальный `id`.

### Нарушает SRP

Один класс `Employee` хранит всю логику в себе.

{{< tabgroup >}}
{{< tab name="Employee" >}}

```java
public class Employee {
	private String name;
	private double experienceInYears;

	public Employee(String name, double experienceInYears) {
		this.name = name;
		this.experienceInYears = experienceInYears;
	}

	public boolean isSenior() {
		return experienceInYears > 5;
	}

	public String generateId() {
		int random = new Random().nextInt(1000);
		return name + random;
	}

	@Override
	public String toString() {
		return "Employee{" +
				"name='" + name + '\'' +
				", experienceInYears=" + experienceInYears +
				'}';
	}
}
```

{{< /tab >}}

{{< tab name="Client" >}}

```java
public class Client {

	public static void main(String[] args) {
		Employee employee = new Employee("Nikita", 2);

		String generatedId = employee.generateId();
		boolean isSenior = employee.isSenior();
		String info = employee.toString();
	}
}
```

{{< /tab >}}
{{< /tabgroup >}}

### Соответствует SRP

Вынести методы определения квалификации и генерации `id` в отдельные классы.

{{< tabgroup >}}
{{< tab name="Employee" >}}

```java
public class Employee {
	private String name;
	private double experienceInYears;

	public Employee(String name, double experienceInYears) {
		this.name = name;
		this.experienceInYears = experienceInYears;
	}

	public String getName() {
		return name;
	}

	public double getExperienceInYears() {
		return experienceInYears;
	}

	@Override
	public String toString() {
		return "Employee{" +
				"name='" + name + '\'' +
				", experienceInYears=" + experienceInYears +
				'}';
	}
}
```

{{< /tab >}}

{{< tab name="ExperienceChecker" >}}

```java
public class ExperienceChecker {

	public static boolean isSenior(double experienceInYears) {
		return experienceInYears > 5;
	}
}
```

{{< /tab >}}

{{< tab name="EmployeeIdGenerator" >}}

```java
public class EmployeeIdGenerator {

	public static String generateId(String name) {
		int random = new Random().nextInt(1000);
		return name + random;
	}
}

```

{{< /tab >}}

{{< tab name="Client" >}}

```java
public class Client {

	public static void main(String[] args) {
		Employee employee = new Employee("Nikita", 2);

		String generatedId = EmployeeIdGenerator.generateId(employee.getName());
		boolean isSenior = ExperienceChecker.isSenior(employee.getExperienceInYears());
		String info = employee.toString();
	}
}
```

{{< /tab >}}
{{< /tabgroup >}}

## 2. Принцип открытости/закрытости (OCP)

{{< tabgroup >}}

{{< tab name="Student" >}}

```java
public class Student {

	private String name;
	private String department;
	private double score;

	public Student(String name, String department, double score) {
		this.name = name;
		this.department = department;
		this.score = score;
	}

	public String getName() {
		return name;
	}

	public String getDepartment() {
		return department;
	}

	public double getScore() {
		return score;
	}

	@Override
	public String toString() {
		return "Student{" +
				"name='" + name + '\'' +
				", department='" + department + '\'' +
				", score=" + score +
				'}';
	}
}
```

{{< /tab >}}

{{< tab name="DistinctionDecider" >}}

```java
public class DistinctionDecider {

	private List<String> science = List.of("Comp.Sc.", "Physics");
	private List<String> arts = List.of("History", "English");

	public boolean hasDistinction(Student student) {
		if (science.contains(student.getDepartment()) && (student.getScore() > 80)) {
			return true;
		}
		if (arts.contains(student.getDepartment()) && (student.getScore() > 70)) {
			return true;
		}
		return false;
	}
}
```

{{< /tab >}}

{{< tab name="Client" >}}

```java
public class Client {

	public static void main(String[] args) {
		List<Student> students = getStudentsForExample();

		DistinctionDecider decider = new DistinctionDecider();
		for (Student student : students) {
			decider.hasDistinction(student);
		}
	}

	private static List<Student> getStudentsForExample() {
		Student sam = new Student("Sam", "Comp.Sc.", 81.5);
		Student bob = new Student("Bob", "Physics", 72);
		Student john = new Student("Sam", "History", 71);
		Student kate = new Student("Sam", "English", 66.5);

		return List.of(sam, bob, john, kate);
	}
}
```

{{< /tab >}}

{{< /tabgroup >}}

{{< tabgroup >}}

{{< tab name="Student" >}}

```java
public abstract class Student {

	private String name;
	private String department;
	private double score;

	protected Student(String name, String department, double score) {
		this.name = name;
		this.department = department;
		this.score = score;
	}

	public String getName() {
		return name;
	}

	public String getDepartment() {
		return department;
	}

	public double getScore() {
		return score;
	}

	@Override
	public String toString() {
		return "Student{" +
				"name='" + name + '\'' +
				", department='" + department + '\'' +
				", score=" + score +
				'}';
	}
}
```

{{< /tab >}}

{{< tab name="ScienceStudent" >}}

```java
public class ScienceStudent extends Student {

	public ScienceStudent(String name, String department, double score) {
		super(name, department, score);
	}
}
```

{{< /tab >}}

{{< tab name="ArtStudent" >}}

```java
public class ArtStudent extends Student {

	public ArtStudent(String name, String department, double score) {
		super(name, department, score);
	}
}
```

{{< /tab >}}

{{< tab name="DistinctionDecider" >}}

```java
public interface DistinctionDecider {

	boolean hasDistinction(Student student);
}
```

{{< /tab >}}

{{< tab name="ScienceDistinctionDecider" >}}

```java
public class ScienceDistinctionDecider implements DistinctionDecider {

	@Override
	public boolean hasDistinction(Student student) {
		return student.getScore() > 80;
	}
}
```

{{< /tab >}}

{{< tab name="ArtDistinctionDecider" >}}

```java
public class ArtDistinctionDecider implements DistinctionDecider {

	@Override
	public boolean hasDistinction(Student student) {
		return student.getScore() > 70;
	}
}
```

{{< /tab >}}

{{< tab name="Client" >}}

```java
public class Client {

	public static void main(String[] args) {
		List<Student> scienceStudents = getScienceStudentsForExample();
		List<Student> artsStudents = getArtsStudentsForExample();

		DistinctionDecider scienceDistinctionDecider = new ScienceDistinctionDecider();
		DistinctionDecider artsDistinctionDecider = new ArtDistinctionDecider();
		for (Student student : scienceStudents) {
			scienceDistinctionDecider.hasDistinction(student);
		}
		for (Student student : artsStudents) {
			artsDistinctionDecider.hasDistinction(student);
		}
	}

	private static List<Student> getScienceStudentsForExample() {
		Student sam = new ScienceStudent("Sam", "Comp.Sc.", 81.5);
		Student bob = new ScienceStudent("Bob", "Physics", 72);

		return List.of(sam, bob);
	}

	private static List<Student> getArtsStudentsForExample() {
		Student john = new ArtStudent("Sam", "History", 71);
		Student kate = new ArtStudent("Sam", "English", 66.5);

		return List.of(john, kate);
	}
}
```

{{< /tab >}}

{{< /tabgroup >}}
