## Resources
- Repo: https://github.com/google/googletest
- User Guide: https://google.github.io/googletest/
- C++ Unit Testing with Google Test Tutorial (JetBrainsTV): https://www.youtube.com/watch?v=16FI1-d2P4E
- Google C++ Testing, GTest, GMock Framework: https://youtube.com/playlist?list=PL_dsdStdDXbo-zApdWB5XiF2aWpsqzV55

# 1. Simple TEST
- simple in use
- with TestSuiteName and TestName
```c++
#include <gtest/gtest.h>

struct BankAccount
{
	int balance = 0;

	BankAccount()
	{
	}

	explicit BankAccount(const int balance) : balance(balance)
	{
	}
};

TEST(AccountTest, BankAccountStartsEmpty)
{
	BankAccount account;
	EXPECT_EQ(0, account.balance);
}

int main(int argc, char **argv)
{
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```
```
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from AccountTest
[ RUN      ] AccountTest.BankAccountStartsEmpty
[       OK ] AccountTest.BankAccountStartsEmpty (0 ms)
[----------] 1 test from AccountTest (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
```
```
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from AccountTest
[ RUN      ] AccountTest.BankAccountStartsEmpty
/local3/mnt/workspace/shanlee/Scenescape/omni-6357/scenescape/FirstParty/Scenescape/apps/scenescape-offline.cpp:19: Failure
Expected equality of these values:
  1
  account.balance
    Which is: 0
[  FAILED  ] AccountTest.BankAccountStartsEmpty (0 ms)
[----------] 1 test from AccountTest (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 0 tests.
[  FAILED  ] 1 test, listed below:
[  FAILED  ] AccountTest.BankAccountStartsEmpty

 1 FAILED TEST
```

# 2. TEST_F
- Using fixture, instead of newing the object in each test
- with TestClassName and TestName
```c++
#include <gtest/gtest.h>

struct BankAccount
{
	int balance = 0;

	BankAccount()
	{
	}

	explicit BankAccount(const int balance) : balance(balance)
	{
	}
};

struct BankAccountTest : testing::Test
{
    BankAccount* account;

    BankAccountTest()
    {
        account = new BankAccount();
    }

    ~BankAccountTest()
    {
        delete account;
    }
};

TEST_F(BankAccountTest, BankAccountStartsEmpty)
{
	EXPECT_EQ(0, account->balance);
}

int main(int argc, char **argv)
{
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```
```
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from BankAccountTest
[ RUN      ] BankAccountTest.BankAccountStartsEmpty
[       OK ] BankAccountTest.BankAccountStartsEmpty (0 ms)
[----------] 1 test from BankAccountTest (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
```

# 3. TEST_P
- with testing set of parameters
```c++
#include <gtest/gtest.h>

struct BankAccount
{
	int balance = 0;

	BankAccount()
	{
	}

	explicit BankAccount(const int balance) : balance(balance)
	{
	}

    void deposit(int amount)
    {
        balance += amount;
    }

    bool withdraw(int amount)
    {
        if (amount <= balance)
        {
            balance -= amount;
            return true;
        }
        return false;
    }
};

struct BankAccountTest : testing::Test
{
    BankAccount* account;

    BankAccountTest()
    {
        account = new BankAccount();
    }

    ~BankAccountTest()
    {
        delete account;
    }
};

TEST_F(BankAccountTest, BankAccountStartsEmpty)
{
	EXPECT_EQ(0, account->balance);
}

struct account_state
{
    int init_balance;
    int withdraw_amount;
    int final_balance;
    bool success;
};

std::ostream& operator<<(std::ostream& os, const account_state& obj)
{
    return os << "init_balance =" << obj.init_balance
    << " withdraw_amount =" << obj.withdraw_amount
    << " final_balance =" << obj.final_balance
    << " success =" << obj.success;
}

struct WithdrawAccountTest : BankAccountTest, testing::WithParamInterface<account_state>
{
    WithdrawAccountTest()
    {
        account->balance = GetParam().init_balance;
    }
};

TEST_P(WithdrawAccountTest, FinalBalance)
{
    auto as = GetParam();
    EXPECT_EQ(as.success, account->withdraw(as.withdraw_amount));
    EXPECT_EQ(as.final_balance, account->balance);
}

INSTANTIATE_TEST_CASE_P(Default, WithdrawAccountTest,
    testing::Values(
        account_state{100, 50, 50, true},
        account_state{100, 120, 100, false}
    )
);

int main(int argc, char **argv)
{
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```
```
[==========] Running 3 tests from 2 test cases.
[----------] Global test environment set-up.
[----------] 1 test from BankAccountTest
[ RUN      ] BankAccountTest.BankAccountStartsEmpty
[       OK ] BankAccountTest.BankAccountStartsEmpty (0 ms)
[----------] 1 test from BankAccountTest (0 ms total)

[----------] 2 tests from Default/WithdrawAccountTest
[ RUN      ] Default/WithdrawAccountTest.FinalBalance/0
[       OK ] Default/WithdrawAccountTest.FinalBalance/0 (0 ms)
[ RUN      ] Default/WithdrawAccountTest.FinalBalance/1
/local3/mnt/workspace/shanlee/Scenescape/omni-6357/scenescape/FirstParty/Scenescape/apps/scenescape-offline.cpp:89: Failure
Expected equality of these values:
  as.final_balance
    Which is: 1020
  account->balance
    Which is: 100
[  FAILED  ] Default/WithdrawAccountTest.FinalBalance/1, where GetParam() = init_balance =100 withdraw_amount =120 final_balance =1020 success =0 (0 ms)
[----------] 2 tests from Default/WithdrawAccountTest (0 ms total)

[----------] Global test environment tear-down
[==========] 3 tests from 2 test cases ran. (0 ms total)
[  PASSED  ] 2 tests.
[  FAILED  ] 1 test, listed below:
[  FAILED  ] Default/WithdrawAccountTest.FinalBalance/1, where GetParam() = init_balance =100 withdraw_amount =120 final_balance =1020 success =0

 1 FAILED TEST
```
```
[==========] Running 3 tests from 2 test cases.
[----------] Global test environment set-up.
[----------] 1 test from BankAccountTest
[ RUN      ] BankAccountTest.BankAccountStartsEmpty
[       OK ] BankAccountTest.BankAccountStartsEmpty (0 ms)
[----------] 1 test from BankAccountTest (0 ms total)

[----------] 2 tests from Default/WithdrawAccountTest
[ RUN      ] Default/WithdrawAccountTest.FinalBalance/0
[       OK ] Default/WithdrawAccountTest.FinalBalance/0 (0 ms)
[ RUN      ] Default/WithdrawAccountTest.FinalBalance/1
[       OK ] Default/WithdrawAccountTest.FinalBalance/1 (0 ms)
[----------] 2 tests from Default/WithdrawAccountTest (0 ms total)

[----------] Global test environment tear-down
[==========] 3 tests from 2 test cases ran. (0 ms total)
[  PASSED  ] 3 tests.
```
