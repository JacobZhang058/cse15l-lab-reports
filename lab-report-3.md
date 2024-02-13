# Lab Report 2 – File Exploration and Text Analysis from the Command Line (Week 5)
***

## Part 1

Failure-Inducing Test Code:
`````
  List<String> input1;
  @Before
  public void setUp(){
    input1 = new ArrayList<>();
    input1.add("abc");
    input1.add("def");
    input1.add("ghi");
  }
	@Test 
	public void testFilter() {
    input1.add("abcde");
    StringChecker abc = new StringChecker_abc();
    List<String> expected = new ArrayList<>();
    expected.add("abc");
    expected.add("abcde");
    List<String> actual = ListExamples.filter(input1, abc);
    assertArrayEquals(expected.toArray(), actual.toArray());
	}

...

class StringChecker_abc implements StringChecker{
  String checkString = "abc";
  public boolean checkString(String s){    return s.contains(checkString);   }
}
`````

Non-Failure-Inducing Test Code:
`````
  List<String> input1;
  @Before
  public void setUp(){
    input1 = new ArrayList<>();
    input1.add("abc");
    input1.add("def");
    input1.add("ghi");
  }
  @Test 
	public void testFilter2() {
    StringChecker abc = new StringChecker_abc();
    List<String> expected = new ArrayList<>();
    expected.add("abc");
    List<String> actual = ListExamples.filter(input1, abc);
    assertArrayEquals(expected.toArray(), actual.toArray());
	}

...

class StringChecker_abc implements StringChecker{
  String checkString = "abc";
  public boolean checkString(String s){    return s.contains(checkString);   }
}
`````

Symptom:
![Image](lab-report-3a.png)

### The Fix
Before:
`````
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }
`````
After:
`````
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(s);
      }
    }
    return result;
  }
`````
Before, the method was adding the checked strings to the beginning of the list, when it is required for the strings in the returned list be in the same order as they appear in the list parameter. 
By altering ```result.add(0, s);``` to ```result.add(s);```, the method will now add the strings in the intended order.
