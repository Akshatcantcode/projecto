# projecto
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.canvas.Canvas;
import javafx.scene.canvas.GraphicsContext;
import javafx.scene.input.MouseEvent;
import javafx.scene.layout.HBox;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.stage.Stage;

import java.util.Random;

public class SustainabilityGame extends Application {
    private int treesPlanted = 0;
    private int adRevenue = 0;
    
    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Sustainability Game");

        // Create a Canvas for the game area
        Canvas gameCanvas = new Canvas(600, 400);
        GraphicsContext gc = gameCanvas.getGraphicsContext2D();

        // Create the side panel for ads
        HBox hbox = new HBox();
        hbox.setSpacing(20);
        hbox.getChildren().add(gameCanvas);
        
        // Ad Placeholder
        AdPanel adPanel = new AdPanel();
        hbox.getChildren().add(adPanel.createAdPlaceholder());

        // Game Logic
        gameCanvas.setOnMouseClicked((MouseEvent event) -> {
            plantTree(gc, event.getX(), event.getY());
        });

        // Set up the scene and the stage
        Scene scene = new Scene(hbox, 900, 400);
        primaryStage.setScene(scene);
        primaryStage.show();

        // Start the game
        startGame(gc);
    }

    private void startGame(GraphicsContext gc) {
        // Initial instructions
        gc.setFill(Color.BLACK);
        gc.setFont(new Font(20));
        gc.fillText("Click to plant trees and save the environment!", 50, 50);
        gc.fillText("Trees Planted: " + treesPlanted, 50, 80);
        gc.fillText("Ad Revenue: $" + adRevenue, 50, 110);
    }

    private void plantTree(GraphicsContext gc, double x, double y) {
        Random rand = new Random();
        // Plant a tree image (represented as a green circle here)
        gc.setFill(Color.GREEN);
        gc.fillOval(x - 10, y - 10, 20, 20);

        // Increase trees planted count
        treesPlanted++;

        // Simulate revenue generation
        adRevenue += rand.nextInt(5) + 1;  // Random revenue per tree planted (between $1 and $5)

        // Redraw the game info
        gc.clearRect(0, 0, 600, 400);
        startGame(gc);
    }

    // Ad Panel class to simulate ad placeholders
    class AdPanel {
        public javafx.scene.layout.StackPane createAdPlaceholder() {
            javafx.scene.layout.StackPane adPlaceholder = new javafx.scene.layout.StackPane();
            adPlaceholder.setStyle("-fx-background-color: #ccc; -fx-alignment: center;");
            adPlaceholder.setMinSize(250, 400);
            javafx.scene.text.Text adText = new javafx.scene.text.Text("Ad Placeholder");
            adText.setFont(new Font(18));
            adPlaceholder.getChildren().add(adText);
            return adPlaceholder;
        }
    }
}
