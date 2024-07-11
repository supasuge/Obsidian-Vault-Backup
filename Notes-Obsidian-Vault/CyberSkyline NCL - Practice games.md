###### Source Code
```ruby
flag = [ 210, 202, 216, 172, 198, 196, 204, 210, 172, 176, 181, 184, 180 ]

def check_flag?(a, flag)
  return false if a.length < flag.length
  begin
    (a.chars.map.with_index { |c, i| c.unpack("U*")[0] ^ 129 == flag[i] }).all? { |c| c }
  rescue
    false
  end
end

puts "What's your guess for the flag?"
ans = check_flag?(gets.strip, flag)
(puts "You got the flag!"; exit) if ans
puts "Not quite..."
```

Q1: What language is the program written in? `ruby`
Q2: What data type does `check_flag?` return? `boolean`
Q3: What is the flag hidden in the program?  `SKY-GEMS-1495`
###### Flag Decoding Answer Source Code
```
# Given flag array and the operation used to check the flag

flag_encoded = [210, 202, 216, 172, 198, 196, 204, 210, 172, 176, 181, 184, 180]

xor_value = 129



# Decoding the flag by reversing the operation (XOR with 129)

flag_decoded = [chr(num ^ xor_value) for num in flag_encoded]

flag = ''.join(flag_decoded)
print(f"Flag: {flag}\n")
```

## Explaination
The program you've provided is written in Ruby. This can be identified by the syntax used, such as `def` for defining methods, the use of `?` in method names (which is a common Ruby convention for methods that return boolean values), and the use of `puts` for printing output to the console.

### Answer to Q1:

The program is written in **Ruby**.

### Answer to Q2:

The `check_flag?` method returns a `boolean` data type (`true` or `false`). This is deduced from the method's implementation where it explicitly returns `false` in certain conditions and the use of the `.all?` method, which also returns a `boolean` value.

### Answer to Q3:

To find the flag hidden in the program, we can reverse the operation applied in the comparison inside the `check_flag?` method. The flag is checked against an input string `a`, character by character, by XOR'ing each character's Unicode code point with `129` and comparing the result to the corresponding value in the `flag` array. To reverse this operation and find the flag, we can XOR each number in the `flag` array with `129` and convert the resulting numbers back to characters.