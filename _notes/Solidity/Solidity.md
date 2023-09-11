
[[인터페이스 및 변수 선언]]

생성자 및 기본 함수

소유권 이전 함수

메타 트렌젝션 함수

1. **인터페이스 및 변수 선언**:

```solidity
interface IERC20 {
    ...
}

contract MultiTransfer {
    address public owner;
    bytes32 private constant META_TRANSACTION_TYPEHASH = ...;
    bytes32 private constant DOMAIN_TYPEHASH = ...;
    bytes32 private domainSeparator;
    mapping(address => uint256) private nonces;
    ...
}

```

- **`IERC20`**는 ERC20 토큰의 기본 함수들을 정의하는 인터페이스입니다.
- **`owner`**는 컨트랙트의 소유자 주소를 저장합니다.
- **`META_TRANSACTION_TYPEHASH`**와 **`DOMAIN_TYPEHASH`**는 EIP-712 타입의 메시지를 위한 상수 해시값입니다.
- **`domainSeparator`**는 EIP-712 도메인 구분자를 저장합니다.
- **`nonces`**는 각 사용자 주소별 nonce 값을 저장하는 매핑입니다.

2. **생성자 및 기본 함수**:

```solidity
constructor() payable {
    owner = msg.sender;
    domainSeparator = keccak256(...);
}

modifier onlyOwner() {
    require(msg.sender == owner, "You are not the owner");
    _;
}

function deposit() external payable {}

function withdraw() external onlyOwner {
    payable(owner).transfer(address(this).balance);
}

```

- 생성자에서는 컨트랙트의 소유자를 설정하고 **`domainSeparator`**를 초기화합니다.
- **`onlyOwner`**는 컨트랙트의 소유자만 함수를 호출할 수 있도록 하는 수정자입니다.
- **`deposit`**는 컨트랙트에 이더를 입금하는 함수입니다.
- **`withdraw`**는 컨트랙트의 소유자만 이더를 출금할 수 있는 함수입니다.

3. **소유권 이전 함수**:

```solidity
address public pendingOwner;

event OwnershipTransferInitiated(...);
event OwnershipTransferred(...);

function transferOwnership(address newOwner) public onlyOwner {...}

function claimOwnership() public {...}

```

- **`pendingOwner`**는 새로운 소유자의 주소를 임시로 저장하는 변수입니다.
- **`transferOwnership`**는 소유권 이전을 초기화하는 함수입니다.
- **`claimOwnership`**는 새로운 소유자가 소유권을 승인하는 함수입니다.

4. **메타 트랜잭션 함수**:

```solidity
function executeMetaTransaction(address userAddress, bytes memory functionSignature, bytes32 sigR, bytes32 sigS, uint8 sigV) public returns(bytes memory) {...}

function getNonce(address user) external view returns(uint256) {...}

```

- **`executeMetaTransaction`**는 메타 트랜잭션을 실행하는 함수입니다. 사용자 주소, 함수 서명, 그리고 서명의 R, S, V 값을 인자로 받습니다.
- **`getNonce`**는 주어진 사용자 주소의 nonce 값을 반환하는 함수입니다.

5. **다중 전송 함수**:

```solidity
function multiSendToken(address token, address[] memory recipients, uint256[] memory amounts) public {...}

function multiSendEther(address[] memory recipients, uint256[] memory amounts) public payable {...}

```

- **`multiSendToken`**은 여러 주소로 ERC20 토큰을 전송하는 함수입니다.
- **`multiSendEther`**는 여러 주소로 이더를 전송하는 함수입니다.

6. **잔액 조회 함수**:

```solidity
function getBalance(address tokenAddress, address account) external view returns (uint256) {...}

```

- **`getBalance`**는 주어진 토큰 주소와 계정 주소의 잔액을 반환하는 함수입니다.

이 컨트랙트는 다양한 기능을 제공합니다: 이더와 ERC20 토큰의 다중 전송, 소유권 이전, 그리고 메타 트랜잭션 실행. 메타 트랜잭션은 사용자가 가스비를 지불하지 않고 트랜잭션을 실행할 수 있게 해줍니다.