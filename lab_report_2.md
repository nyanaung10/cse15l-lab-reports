# Servers and Bugs

##Part 1
Code: 
`import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Number: %d", num);
        } else if (url.getPath().equals("/increment")) {
            num += 1;
            return String.format("Number incremented!");
        } else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("count")) {
                    num += Integer.parseInt(parameters[1]);
                    return String.format("Number increased by %s! It's now %d", parameters[1], num);
                }
            }
            return "404 Not Found!";
        }
    }
}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}`

Methods called: 
  * Integer.parseInt()
Value of the relevant arguments: 
  * parameters[]

##PART 2

###ReverseInPlace 
failure-inducing input: 
` @Test 
	public void testReverseInPlace1() {
    int[] input1 = { 1,2,3,4,5,6,7 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 7,6,5,4,3,2,1 }, input1);
}`
symptom: 
  * arrays first differed at element [4]; expected:<3> but was:<5>
bug: 
  * The bug of reverseInPlace function was that the elements in arr is continuously changed throughout the loop. Hence, the code could not find the right element to use to reverse
code fix: 
`
  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    int newArr[] = new int [arr.length];

    for(int i = 0; i < arr.length; i += 1) {
      newArr[i] = arr[i];
    }

    for (int i = 0; i < arr.length; i++) {
      arr[i] = newArr[arr.length - i - 1];
    }
  }
`


##Reverse
failure-inducing input: 
`
@Test
  public void testReversed1() {
    int[] input1 = { 1,2,3,4,5};
    assertArrayEquals(new int[]{5,4,3,2,1 }, ArrayExamples.reversed(input1));
  }
}
`
symptom: 
  * arrays first differed at element [0]; expected:<5> but was: <0>
Bug: Ignore doubles that are equal to the lowest value 
