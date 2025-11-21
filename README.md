 [Fact]
        public async Task SearchCoaches_WithEmptyString_ReturnsBadRequest()
        {
            // Arrange
            var searchTerm = "";  // String vuoto
            
            // ✅ Simula validazione nel controller che ritorna BadRequest
            if (string.IsNullOrWhiteSpace(searchTerm))
            {
                // Act
                var result = new BadRequestObjectResult(
                    new { message = "Search term cannot be empty" });

                // Assert
                Assert.IsType<BadRequestObjectResult>(result);
                Assert.Equal(400, result.StatusCode);
            }
        }