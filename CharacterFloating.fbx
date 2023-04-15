// Declaração das variáveis
float speed = 10.0f; // Velocidade de movimento
float jumpHeight = 500.0f; // Altura do pulo
bool isGrounded = true; // Verifica se o personagem está no chão

// Referência para o carro
AActor* carReference;

// Função para movimentar o personagem
void Move(float ValueX, float ValueY)
{
    // Calcula a direção do movimento
    FVector Direction = FVector(ValueX, ValueY, 0.f).GetSafeNormal();

    // Move o personagem na direção calculada
    FVector Movement = (Direction * speed * GetWorld()->GetDeltaSeconds());
    AddMovementInput(Movement);
}

// Função para pular
void Jump()
{
    // Verifica se o personagem está no chão antes de pular
    if (isGrounded)
    {
        // Calcula a força do pulo
        FVector JumpForce = FVector(0.f, 0.f, jumpHeight);

        // Aplica a força do pulo ao personagem
        LaunchCharacter(JumpForce, false, false);
        isGrounded = false;
    }
}

// Função para sentar no carro
void SitInCar()
{
    // Verifica se o carro existe
    if (carReference != nullptr)
    {
        // Entra no carro
        GetMovementComponent()->StopMovementImmediately();
        AttachToActor(carReference, FAttachmentTransformRules::SnapToTargetNotIncludingScale);
    }
}

// Função para sair do carro
void GetOutCar()
{
    // Verifica se o carro existe
    if (carReference != nullptr)
    {
        // Sai do carro
        DetachFromActor(FDetachmentTransformRules::KeepWorldTransform);
    }
}

// Função para detectar quando o personagem toca o chão
void OnLanded(const FHitResult& Hit)
{
    isGrounded = true;
}

// Função chamada a cada frame
void Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // Detecta a entrada do jogador
    float ValueX = InputComponent->GetAxisValue("MoveForward");
    float ValueY = InputComponent->GetAxisValue("MoveRight");

    // Movimenta o personagem com base na entrada do jogador
    Move(ValueX, ValueY);

    // Detecta se o jogador pressionou a tecla de pulo
    if (InputComponent->IsPressed("Jump"))
    {
        Jump();
    }

    // Detecta se o jogador pressionou a tecla de sentar no carro
    if (InputComponent->IsPressed("SitInCar"))
    {
        SitInCar();
    }

    // Detecta se o jogador pressionou a tecla de sair do carro
    if (InputComponent->IsPressed("GetOutCar"))
    {
        GetOutCar();
    }
}

// Função chamada quando o personagem é inicializado
void BeginPlay()
{
    Super::BeginPlay();

    // Configura a função de detecção de toque no chão
    GetCharacterMovement()->OnLanded.AddDynamic(this, &MyCharacter::OnLanded);

    // Encontra o carro na cena
    TArray<AActor*> FoundActors;
    UGameplayStatics::GetAllActorsOfClass(GetWorld(), ACar::StaticClass(),
